#jhpce01.jhsph.edu
#qrsh -l mf=4G,h_vmem=8G 

x<-c('dplyr','data.table','stringr','zoo')
lapply(x, require, character.only=T)

setwd('/home/bst/other/kebisu/BW_SW')

df=read.csv('Data/calibration2004_2009_scaled_noneg.csv') 
#df2=df %>%
#	mutate(.,FIPS_County=sprintf('%05d',fips),Date=as.Date(paste(as.numeric(str_split_fixed(date,"/",3)[,3]),as.numeric(str_split_fixed(date,"/",3)[,1]),as.numeric(str_split_fixed(date,"/",3)[,2]),sep='-'),'%Y-%m-%d'),calibrated_ambPM=fitted-calibrated_firePM) %>%
#	select(.,FIPS_County,Date,PM25_orig,PM25amb,totalpm,fitted,calibrated_firePM,calibrated_ambPM,PM25monitor)
#save(df2,file='FFData.RData')

load('FFData.RData')
df2=data.table(df2)
setkey(df2,FIPS_County,Date)

df2=df2[CJ(unique(FIPS_County),seq(min(Date),max(Date),by=1))]
df2[,ForestFire_Week:=rollapply(calibrated_firePM,7,mean,align=c("right"),fill=NA,na.rm=TRUE),by=FIPS_County]
df2[,Amb_Week:=rollapply(calibrated_ambPM,7,mean,align=c("right"),fill=NA,na.rm=TRUE),by=FIPS_County]
df2[,PM25_Week:=rollapply(fitted,7,mean,align=c("right"),fill=NA,na.rm=TRUE),by=FIPS_County]


df2$ForestFire_Week[is.nan(df2$ForestFire_Week)]=NA
df2$Amb_Week[is.nan(df2$Amb_Week)]=NA
df2$PM25_Week[is.nan(df2$PM25_Week)]=NA

load('/home/bst/other/kebisu/BW_PMCoarse/Data/bwdata_orig.RData')
yr_use=c(2004:2007)
bwdata=filter(bwdata,State.Code=='06',substr(BD_Est,1,4) %in% yr_use,dgestat==39) 
