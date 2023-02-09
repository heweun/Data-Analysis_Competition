setwd("C:\\Users\\hoeeun\\Desktop\\학술제")
library(sp)
library(rgdal)
library(ggmap)
library(rgeos)
library(maptools)
library(spatstat)
library(sf)

head(apt_buy)
names(apt_buy)[8]<-"Longitude"
names(apt_buy)[9]<-"Latitude"

pt<-data.frame(longitude=A_m$Longitude,latitude=A_m$Latitude)
str(pt)

cs<-CRS("+proj=longlat +datum=WGS84")
spt<-SpatialPoints(pt,proj4string = cs)
buy<-SpatialPointsDataFrame(spt,data=A_m)

plot(buy)

##
admin<-readOGR(dsn="C:\\Users\\hoeeun\\Desktop\\학술제",
               layer = "TL_SCCO_SIG_W")
proj4string(admin)=cs
plot(admin,axes=T)
plot(buy,group="지역코드",add=T)
title("서울시 대피소 위치")


##
register_google(key ='AIzaSyBbWWBZn60hG-ldcbwcsgKurXvaw6rZFRs')
gmap<-get_map(location = c(lon=127,lat=37.55),
              zoom = 11,
              maptype = "roadmap")
mapdata<-data.frame(buy)

ggmap(gmap)+
  geom_point(data = mapdata,
                       aes(x=longitude,y=latitude,color=factor(y)),
                       alpha=1)+
  geom_polygon(data = new_map, aes(x=long, y=lat, group=group),
               fill=NA,color="black")

ggplot()+
  geom_point(data = mapdata,
             aes(x=longitude,y=latitude,color=factor(y)),
             alpha=1)+
  geom_polygon(data = seoul_map, aes(x=long, y=lat, group=group),
               fill=NA,color="black")+
  theme_bw()+
  geom_text(data = code2,
            aes(x = 경도,y = 위도,label = paste(시군구명)),size=3)+
  ggtitle("매매여부")+
  theme(plot.title = element_text(hjust = 0.5))+
  scale_color_discrete(name="매매여부", 
                       breaks=c(1,0),
                       labels=c("가능","불가"))

##
cs2<-CRS("+proj=tmerc +lat_0=38 +lon_0=127.5 +k=0.9996 +x_0=1000000 +y_0=2000000 +ellps=GRS80 +units=m")
buy_utm<-spTransform(buy,cs2)
admin_utm<-spTransform(admin,cs2)

buy_buf<-gBuffer(buy_utm,width = 300)
plot(buy_buf,axes=T)
plot(buy_utm,col=3,add=T)


##
apt_rent<-read.csv("C:/Users/hoeeun/Desktop/학술제/서울전세_final.csv")
names(apt_rent)[9]<-"Longitude"
names(apt_rent)[10]<-"Latitude"
apt_rent<-apt_rent%>%
  filter(!is.na(Longitude))


pt<-data.frame(longitude=apt_rent$Longitude,latitude=apt_rent$Latitude)

cs<-CRS("+proj=longlat +datum=WGS84")
spt<-SpatialPoints(pt,proj4string = cs)
rent<-SpatialPointsDataFrame(spt,data=apt_rent)

##
rent_utm<-spTransform(rent,cs2)
rent_buf<-gBuffer(rent_utm,width = 300)

s_d_intersect<-gIntersection(buy_buf,rent_buf)

plot(admin_utm,axes=T,xaxt = "n",yaxt="n")
plot(s_d_intersect,col=5,add=T)
plot(buy_buf,add=T)
title("전세& 매매")

rm(apt_rent1,apt_rent2,apt_rent3,apt_rent4,apt_rent5,apt_rent6)

#데이터합치기
A<-apt_rent[!duplicated(apt_rent[,c('Longitude','Latitude')]),]
A<-A%>%
  dplyr::select(Longitude,Latitude,score)
A_m<-merge(A,apt_buy,by=c('Longitude','Latitude'),all=T)
A_m<-A_m%>%
  filter(!is.na(지역코드))
A_m$score<-ifelse(is.na(A_m$score),7,A_m$score)

table(is.na(A_m))
write.csv(A_m,file = "C:\\Users\\hoeeun\\Desktop\\학술제\\data.csv")

#######
s<-readOGR("C:\\Users\\hoeeun\\Desktop\\R을 이용한 공간정보 분석_실습자료\\ch7",
           "Seoul")
gu<-as(s,"owin")

s<-readOGR("C:\\Users\\hoeeun\\Desktop\\R을 이용한 공간정보 분석_실습자료\\ch7",
           "Seoul")

pt<-data.frame(longitude=A_m$Longitude,latitude=A_m$Latitude)
str(pt)
cs<-CRS("+proj=longlat +datum=WGS84")
spt<-SpatialPoints(pt,proj4string = cs)
G_A_m<-SpatialPointsDataFrame(spt,data=A_m)

Window(G_A_m)<-admin

s<-readOGR(dsn="C:/Users/hoeeun/Desktop/학술제",
           layer="final2")
cs<-CRS(SRS_string ="EPSG:4326" )
proj4string(s)=cs

subuck<-as(s,"ppp")
