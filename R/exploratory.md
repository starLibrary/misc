```R


View(baltimore)
unique(baltimore$type)
View(scc2)
View(SCC[SCC$EI.Sector %in% sources,])





unique(NEI$type)
px <- SCC[   SCC$SCC  %in%  NEI[ NEI$type=="ON-ROAD", ]$SCC,   ]
View(SCC)
unique(px$Data.Category)
View(px)
length(unique( SCC[ SCC$Data.Category=="Onroad", ]$EI.Sector ))







library(ggplot2)
total_emissions <- dplyr::summarise(  dplyr::group_by(NEI[(NEI$fips=="24510") & (NEI$type=="ON-ROAD"),], year), Emissions=sum(Emissions)  )
ggplot(data=total_emissions, mapping = aes(x=factor(year),y=Emissions)) +
        geom_bar(stat="identity")+
        geom_label(mapping = aes(x=factor(year),y=Emissions,label=round(Emissions,2)))+
        xlab("Years")+
        ylab(expression('Baltimore City PM'[2.5]*' emissions in tons'))+
        ggtitle(label=expression('Baltimore City PM'[2.5]*' emissions in tons from 1999 to 2008, faceted by emission type'))+
        theme(plot.title = element_text(hjust = 0.5))


###############################################################################














## This first line will likely take a few seconds. Be patient!
system.time(
NEI <- readRDS("summarySCC_PM25.rds")
)
# View(NEI)
SCC <- readRDS("Source_Classification_Code.rds")
# View(SCC)



unique(SCC$Option.Group)
unique(SCC$Option.Set)
unique(SCC$SCC.Level.One)
unique(SCC$SCC.Level.Two)
unique(SCC$SCC.Level.Three)
unique(SCC$SCC.Level.Four)
unique(SCC$Map.To)
unique(SCC$Last.Inventory.Year)



years <- unique(NEI$Pollutant)

t <- SCC[grepl("Coal", SCC$EI.Sector),]
unique(SCC$Short.Name)
unique(SCC$EI.Sector)
unique(t$EI.Sector)

unique()

emissions <-  c(
        sum(     NEI[  NEI$year == years[1]  ,  ]$Emissions     ),
        sum(     NEI[  NEI$year == years[2]  ,  ]$Emissions     ),
        sum(     NEI[  NEI$year == years[3]  ,  ]$Emissions     ),
        sum(     NEI[  NEI$year == years[4]  ,  ]$Emissions     )
)




par(bg="white")
plot( years, emissions/1000, type="l", xlab="years", ylab=expression('total PM'[2.5]*' emission'))
points( years, emissions/1000, pch=19, col = "blue")





# Checking for NA values

sum(is.na(  NEI$year  ))

sum(is.na(  NEI$Emissions  ))


# PLOT
par(bg="#E6E6E6")
## par(fg="white")
total_emissions <- dplyr::summarise(  dplyr::group_by(NEI, year), Emissions=sum(Emissions)  )
plot <- barplot(height = total_emissions$Emissions/1000,
            xlab="time in years", ylab=expression('total PM'[2.5]*' emissions in kilo-tons'),
            ylim=c(0,8100),
            names.arg = total_emissions$year,
            main=expression('Total PM'[2.5]*' emissions in kilo-tons'),col=c("#9403BE","#7903B8","#7903B8"))

## Adding values above the bars#B97DE4#56177A#7903B8#D742C2#F70080#9403BE#73009A#CF73DD
text(x = plot,
     y = round(total_emissions$Emissions/1000,6),
     label = round(total_emissions$Emissions/1000,3), pos = 3, cex = 0.8, col = "black", font=2)







sum(is.na(  NEI$fips  ))
sum(  NEI$fips==" "  )
sum(  NEI$fips==""  )


# PLOT
par(bg="#E6E6E6")
## par(fg="white")
baltimore <- NEI[NEI$fips=="24510",]
total_emissions <- dplyr::summarise(  dplyr::group_by(baltimore, year), Emissions=sum(Emissions)  )
plot <- barplot(height = total_emissions$Emissions,
                xlab="time in years", ylab=expression('total PM'[2.5]*' emissions in tons'),
                ylim=c(0,3500),
                names.arg = total_emissions$year,
                main=expression('Total PM'[2.5]*' emissions in tons'),col=c("#7903B8","#9403BE"))

text(x = plot,
     y = round(total_emissions$Emissions,6),
     label = round(total_emissions$Emissions,3), pos = 3, cex = 0.8, col = "black", font=2)







class(NEI$year)
NEI[1,6] <- NA

x <- SCC[SCC$SCC %in% "10100101",]

unique(SCC$Data.Category)
sum(is.na(SCC$Data.Category))
sum(" "==SCC$Data.Category)


for(i in 1:dim(SCC)[2]){
        x <- paste(i,":",sum(is.na(SCC[,i])),sum(""==SCC[,i]), sum(" "==SCC[,i]),"::",
                   sum(is.na(SCC[,i])) + sum(""==SCC[,i]) + sum(" "==SCC[,i])   )
        print(x)
}

i = 6
for(i in 1:dim(NEI)[2]){
        x <- paste(i,":",sum(is.na(NEI[,i])),sum(""==NEI[,i]), sum(" "==NEI[,i]),"::",
                   sum(is.na(NEI[,i])) + sum(""==NEI[,i]) + sum(" "==NEI[,i])   )
        print(x)
}



x[1]
x[2]
x[3]
x[4]
x[5]
x[6]
x[7]
x[8]
x[9]
x[10]
x[11]
x[12]
x[13]
x[14]
x[15]




str(NEI)
summary(NEI)

str(SCC)
summary(SCC)

neii <- as.factor(NEI$year)

unique(neii)


#####################################################################################


2075259 * 9 * 8




system.time(
        initial <- readr::read_csv2("household_power_consumption.txt",
                                    col_types = readr::cols(
                                                      Date = col_date(),
                                                      Time = col_time(),
                                                      Global_active_power = col_double(),
                                                      Global_reactive_power = col_double(),
                                                      Voltage = col_double(),
                                                      Global_intensity = col_double(),
                                                      Sub_metering_1 = col_double(),
                                                      Sub_metering_2 = col_double(),
                                                      Sub_metering_3 = col_double()
                                                      ),
                                    na = c("", "NA", "?"),
                                    progress = FALSE)
        initial <- readr::read_csv2("household_power_consumption.txt",
                                    na = c("??"),
                                    progress = FALSE)
        #initial <- readr::read_delim("household_power_consumption.txt", delim = ";", na = c("", "NA", "?"), progress = FALSE)
)
# View(initial)
final <- initial[initial$Date %in% c("1/2/2007","2/2/2007"),]
final <- initial[final[,1] %in% c("1/2/2007","2/2/2007"),]
# View(final)
system.time(
        initial <- read.table("household_power_consumption.txt", header=TRUE, sep=";", stringsAsFactors=FALSE)
)











NA_STRINGS <- c("", "NA", "?")
colm_types <- c("D","t","d","d","d","d","d","d","d")

ROWS_TO_READ <- 200000
file_remaining <- TRUE
SKIP_LINES <- 1
final <- readr::read_csv2("household_power_consumption.txt", skip = 1, n_max = 1000,
                          na = NA_STRINGS, col_names = FALSE, quoted_na = FALSE,
                          progress = FALSE )
print(final)
final <- final[final$X1 %in% c("1/2/2007","2/2/2007"),]
SKIP_LINES <- 1001

while(file_remaining){
        initial <- readr::read_csv2( "household_power_consumption.txt", skip = SKIP_LINES,
                                     n_max = ROWS_TO_READ, na = NA_STRINGS,
                                     col_names = FALSE, quoted_na = FALSE, progress = FALSE )
        
        if(   nrow(initial) < ROWS_TO_READ   ){
                file_remaining <- FALSE
        }
        
        initial <- initial[initial$X1 %in% c("1/2/2007","2/2/2007"),]
        final <- dplyr::bind_rows(final,initial)
        SKIP_LINES <- SKIP_LINES + ROWS_TO_READ
}






colnames(final) <- final[1,]
final <- final[-1,]

final <- readr::type_convert(final)


























final$Date[2001:3000]

initial <- read.table("household_power_consumption.txt", header=TRUE, sep=";", nrows = 500, stringsAsFactors=FALSE)
View(initial)

sapply(initial, class)


sum(sapply(initial[1,], object.size))*2075259/1000
classes <- sapply(initial, class)
tabAll <- read.table("household_power_consumption.txt", sep=";", colClasses=classes)


cls <- c("Date",)


read.table()


object.size(initial[1,1])
object.size(initial)
print(object.size(initial[1,]), units = "auto")

sapply(initial[8,], object.size)
print(object.size(initial[8,8]))


class(as.Date(initial[,1])[1])
strptime(initial[,2],format = "%H:%M:%S", tz = "UTC")


strptime(  paste(initial[,1],initial[,2]),  format = "%d/%m/%Y %H:%M:%S",  tz = "UTC")

initial <- read.table("household_power_consumption.txt",
           header=TRUE,
           sep=";",
           skip=grep("2005-12-31", readLines("household_power_consumption.txt")),
           nrows=365)




read_csv2(
        
        file,
        col_names = TRUE, #
        col_types = NULL,
        locale = default_locale(),
        na = c("", "NA"),
        quoted_na = TRUE,
        quote = "\"",
        comment = "",
        trim_ws = TRUE,
        skip = 0,
        n_max = Inf,
        guess_max = min(1000, n_max),
        progress = show_progress(),
        skip_empty_rows = TRUE
        
)

























library(tibble)
class(tibble::as_data_frame(final))



#####################################################################################

pollution <- read.csv("exploratory/avgpm25.csv", colClasses = c("numeric", "character", "factor", "numeric", "numeric"))


DataSet <- pollution

100*colSums( is.na(DataSet) ) / (dim(DataSet)[1])
colSums( is.na(DataSet) )




data(airquality)
head(airquality)
tail(airquality)

# 1 diemensional

summary(airquality$Ozone) # Summary also reports no. of NA's if present, but doesnot detect outliers

        function() NOTE
        100*colSums( is.na(airquality) ) / (dim(airquality)[1]) # Gives % of NA's per column
        colSums( is.na(airquality) ) # Gives counts of NA's per column, useful for small datasets

airquality <- airquality[complete.cases(airquality),]

boxplot(airquality$Ozone, col="blue")
abline(h = 177) # If value is too much beyond the boxplot range, it wont appear

hist(airquality$Ozone, col="green", breaks = 55) # If breaks are too high, hist is noisy, too low, hist is inaccurate
rug(airquality$Ozone)
abline(v = 85, lwd = 2)
abline(v = median(airquality$Ozone), col = "magenta", lwd = 2) # median() wont work if there are NA values, hence no lines will be plotted

str(table(airquality$Month))
barplot(table(airquality$Month), col = "wheat", main = "No of <whatever>")

# 2 diemensional

boxplot(Solar.R ~ Month, data = airquality, col = "red")
summary(airquality)

# par once set remains same until reset, even if you change to another plot
par(mfrow = c(2,1), mar = c(4, 4, 2, 1))
hist(subset(airquality, Month == 8)$Temp, col = "green")
rug(airquality$Temp)
hist(subset(airquality, Month == 5)$Temp, col = "green")
rug(airquality$Temp)

?par
with(airquality, plot(Wind, Ozone))
abline(h = 12, lwd = 2, lty = 2)

airquality <- transform(airquality, Month = factor(Month))
boxplot(Ozone ~ Month, airquality, xlab="Month", ylab="Ozone ppb")




GGPLOT2

library(ggplot2)
str(mpg)
qplot(displ, hwy, data = mpg)
qplot(displ, hwy, data=mpg, color=drv)
qplot(displ, hwy, data=mpg, geom=c("point","smooth"))
qplot(hwy, data=mpg, fill=drv)

qplot(displ, hwy, data=mpg, facets=.~drv)
qplot(hwy, data=mpg, facets = drv~., binwidth=2)


qplot(log(eno), data=maacs)
qplot(log(eno), data=maacs, fill=mopos)
qplot(log(eno), data=maacs, geom="density")
qplot(log(eno), data=maacs, geom="density", color=mopos)
qplot(log(pm25), log(eno), data=maacs)
qplot(log(pm25), log(eno), data=maacs, shape=mopos)
qplot(log(pm25), log(eno), data=maacs, color=mopos)
qplot(log(pm25), log(eno), data=maacs, color=mopos) + geom_smooth(method = "lm")
qplot(log(pm25), log(eno), data=maacs, facets=.~mopos) + geom_smooth(method = "lm")













lattice is an implementation of Trellis graphics for R.

| Lattice is implemented using two packages. The first is called, not
| surprisingly, lattice, and it contains code for producing Trellis graphics.
| Some of the functions in this package are the higher level functions which
| you, the user, would call. These include xyplot, bwplot, and levelplot.

| The second package in the lattice system is grid which contains the low-level
| functions upon which the lattice package is built. You, the user, seldom call
| functions from the grid package directly.

| The lattice system, as the base does, provides several different plotting
| functions. These include xyplot for creating scatterplots, bwplot for
| box-and-whiskers plots or boxplots, and histogram for histograms. There are
| several others (stripplot, dotplot, splom and levelplot), which we wont
| cover here.

xyplot(Ozone~Wind,data=airquality)
xyplot(Ozone~Wind,data=airquality,col="red",pch=8,main="Big Apple Data")--doesnt work

xyplot(Ozone ~ Wind, data = airquality, pch=8, col="red", main="Big Apple Data")

xyplot(Ozone~Wind | as.factor(Month),data=airquality,layout=c(5,1))

xyplot(Ozone~Wind | Month,data=airquality,layout=c(5,1))--same as previous except lables

| Lattice functions behave differently from base graphics functions in one
| critical way. Recall that base graphics functions plot data directly to the
| graphics device (e.g., screen, or file such as a PDF file). In contrast,
| lattice graphics functions return an object of class trellis.

| The print methods for lattice functions actually do the work of plotting the
| data on the graphics device. They return "plot objects" that can be stored
| (but its usually better to just save the code and data). On the command
| line, trellis objects are auto-printed so that it appears the function is
| plotting the data.

p<-xyplot(Ozone~Wind,data=airquality)
names(p)--trellis object p has 45 named properties
p[["formula"]]
p[["x.limits"]]

xyplot(y~x | f,layout=c(2,1))
is same as
xyplot(y~x | f,data=airquality,layout=c(2,1))



p <- xyplot(y ~ x | f, panel = function(x, y, ...) {
panel.xyplot(x, y, ...)  ## First call the default panel function for 'xyplot'
panel.abline(h = median(y), lty = 2)  ## Add a horizontal line at the median
})
print(p)
invisible()

| The panel function has 3 arguments, x, y and ... . This last stands for all
| other arguments (such as graphical parameters) you might want to include.
| There are 2 lines in the panel function. Each invokes a panel method, the
| first to plot the data in each panel (panel.xyplot), the second to draw a
| horizontal line in each panel (panel.abline). Note the similarity of this
| last call to that of the base plotting function of the same name.



p2 <- xyplot(y ~ x | f, panel = function(x, y, ...) {
panel.xyplot(x, y, ...)  ## First call default panel function
panel.lmline(x, y, col = 2)  ## Overlay a simple linear regression line
})
print(p2)
invisible()


table(diamonds$color,diamonds$cut)

xyplot(price~carat | color*cut,data=diamonds,strip=FALSE,pch=20,xlab=myxlab,ylab=myylab,main=mymain)

| Pretty cool, right? 35 panels, one for each combination of color and cut.
| The dots (pch=20) show how prices for the diamonds in each category (panel)
| vary depending on carat.








Swirl : Colors

colors()
sample(colors(),10)
pal <- colorRamp(c("red","blue"))
pal(0)
pal(1)
pal(seq(0,1,len=6))
p1 <- colorRampPalette(c("red","blue"))
p1(6)
0xcc
p2 <- colorRampPalette(c("red","yellow"))
p2(6)
p2(10)
p3 <- colorRampPalette(c("blue","green"),alpha=0.5)

We generated 1000 random normal pairs for you in the variables x and y.
plot(x,y,pch=19,col=rgb(0,.5,.5))
plot(x,y,pch=19,col=rgb(0,.5,.5,.3))
cols <- brewer.pal(3,"BuGn")
showMe(cols)
pal <- colorRampPalette(cols)
image(volcano,col=p1(20))






Swirl : GGPlot2 Part1

qplot(displ,hwy,data=mpg,color=drv,geom=c("point","smooth"))

qplot(y=hwy,data=mpg,color=drv)
qplot(displ,hwy,data=mpg,geom=c("point","smooth"))
# specifying the y parameter only, without an x argument, plots the values of the "y" argument in the order in which they occur in the data.
# but
qplot(hwy,data=mpg,fill=drv)
# plots a histogram

qplot(drv,hwy,data=mpg,geom="boxplot")

qplot(drv,hwy,data=mpg,geom="boxplot",color=manufacturer)

qplot(displ,hwy,data=mpg,facets=.~drv)

qplot(hwy,data=mpg,facets=drv~.,binwidth=2)
qplot(hwy,data=mpg,facets=drv~cyl,binwidth=2)

qplot(displ,hwy,data=mpg,facets=drv~cyl,geom=c("point","smooth"))
# gg in ggplot2 stands for grammar of graphics






library(ggplot2)
ggplot() + geom_point(data=quakes,aes(x=lat,y=long,colour=stations))






Swirl : GGPlot2 Part2

qplot(displ,hwy,data=mpg,geom=c("point","smooth"),facets=.~drv,color=drv)

g <- ggplot(mpg,aes(displ,hwy))
summary(g)
summary(g+geom_point())
summary(g+geom_point()+geom_smooth())
summary(g+geom_point()+geom_smooth(method="lm"))
summary(g+geom_point()+geom_smooth(method="lm")+facet_grid(.~drv))
summary(g+geom_point()+geom_smooth(method="lm")+facet_grid(.~drv)+ggtitle("Swirl Rules"))

Each of the "geom"
| functions (e.g., _point and _smooth) has options to modify it. Also, the function
| theme() can be used to modify aspects of the entire plot, e.g. the position of the
| legend. Two standard appearance themes are included in ggplot. These are theme_gray()
| which is the default theme (gray background with white grid lines) and theme_bw()
| which is a plainer (black and white) color scheme.

summary(g+geom_point(color="pink",size=4,alpha=1/2))

Now we will modify the aesthetics so that color indicates which drv type each point
| represents. Again, use g and add to it a call to the function geom_point with 3
| arguments. The first is size set equal to 4, the second is alpha equal to 1/2. The third
| is a call to the function aes with the argument color set equal to drv. Note that you
| MUST use the function aes since the color of the points is data dependent and not a
| constant as it was in the previous example.

summary(g+geom_point(size=4,alpha=1/2,aes(color=drv)))

summary(g + geom_point(aes(color = drv)) + labs(title="Swirl Rules!") + labs(x="Displacement", y="Hwy Mileage"))

summary(g+geom_point(aes(color=drv),size=2,alpha=1/2)+geom_smooth(size=4,linetype=3,method="lm",se=FALSE))

summary(g + geom_point(aes(color = drv)) + theme_bw(base_family="Times"))


g <- ggplot(data=testdat,aes(x=myx,y=myy))

g + geom_line() + ylim(-3,3)

g + geom_line() + coord_cartesian(ylim=c(-3,3))
See the difference? This looks more like the plot produced by the base plot function. The outlier y value at x=50 is not shown, but the plot indicates that it is larger than 3.


g <- ggplot(data=mpg, aes(x=displ,y=hwy,color=factor(year)))

g+geom_point()+facet_grid(drv~cyl,margins=TRUE)
The margins argument tells ggplot to display the marginal totals over each row and
| column, so instead of seeing 3 rows (the number of drv factors) and 4 columns (the number of cyl
| factors) we see a 4 by 5 display. Note that the panel in position (4,5) is a tiny version of the
| scatterplot of the entire dataset.

g+geom_point()+facet_grid(drv~cyl,margins=TRUE)+geom_smooth(method="lm",se=FALSE,size=2,color="black")

g+geom_point()+facet_grid(drv~cyl,margins=TRUE)+geom_smooth(method="lm",se=FALSE,size=2,color="black")+labs(x="Displacement",y="Highway Mileage",title="Swirl Rules!")






Swirl : GGPlot2 Extras

qplot(price,data=diamonds,binwidth=18497/30)

| No more messages in red, but a histogram almost identical to the previous
| one! If you typed 18497/30 at the command line you would get the result
| 616.5667. This means that the height of each bin tells you how many diamonds
| have a price between x and x+617 where x is the left edge of the bin.

qplot(price,data=diamonds,binwidth=18497/30,fill=cut)
try this with ggplot

| This shows how the counts within each price grouping (bin) are distributed
| among the different cuts of diamonds. Notice how qplot displays these
| distributions relative to the cut legend on the right. The fair cut diamonds
| are at the bottom of each bin, the good cuts are above them, then the very
| good above them, until the ideal cuts are at the top of each bin.

qplot(price,data=diamonds,binwidth=18497/30,fill=cut,facets=.~cut)

qplot(price,data=diamonds,geom="density",color=cut)

qplot(carat,price,data=diamonds)

qplot(carat,price,data=diamonds,shape=cut)

qplot(carat,price,data=diamonds,color=cut)+geom_smooth(method="lm")

qplot(carat,price,data=diamonds,color=cut,facets=.~cut)+geom_smooth(method="lm")



g <- ggplot( diamonds, aes(depth,price) )
g + geom_point(alpha=1/3)

cutpoints <- quantile(diamonds$carat, seq(0,1, length=4), na.rm=TRUE)

diamonds$car2 <- cut(diamonds$carat,cutpoints)

g <- ggplot( diamonds, aes(depth,price) )
g + geom_point(alpha=1/3) + facet_grid(cut~car2)

| The first 3 columns are labeled with the cutpoint boundaries. The fourth is
| labeled NA and shows us where the data points with missing data (NA or Not
| Available) occurred. We see that there were only a handful (12 in fact) and
| they occurred in Very Good, Premium, and Ideal cuts. We created a vector,
| myd, containing the indices of these datapoints. Look at these entries in
| diamonds by typing the expression diamonds[myd,]. The myd tells R what rows
| to show and the empty column entry says to print all the columns.

diamonds[myd,]

The car2 field
| is, in fact, NA for these entries, but the carat field shows they each had a
| carat size of .2. What is going on here?

| Actually our plot answers this question. The boundaries for each column
| appear in the gray labels at the top of each column, and we see that the
| first column is labeled (0.2,0.5]. This indicates that this column contains
| data greater than .2 and less than or equal to .5. So diamonds with carat
| size .2 were excluded from the car2 field.

g + geom_point(alpha=1/3) + facet_grid(cut~car2) + geom_smooth(method="lm",size=3,color="pink")

ggplot( diamonds, aes(carat,price) ) + geom_boxplot() + facet_grid(.~cut)





Swirl : Heirarchical Clustering

dist(dataFrame)
hc <- hclust(distxy)
hc
plot(hc)
plot(as.dendrogram(hc))
abline(h=.4,col="red")
dist(dFsm)
heatmap(dataMatrix, col=cm.colors(25))
heatmap(mt)
plot(denmt)
distmt






















#par(mfrow = c(2,3), bg="grey")
cols <- dim(DataSet)[2]
col_names <- colnames(DataSet)
for (i in 1:cols)
{
        if(  class(DataSet[,i]) %in% c("numeric","integer","factor")  )
        {
                if(   sum( is.na(DataSet[,i]) )   >   0   )
                {
                        dens <- density(   DataSet[,i][!is.na(DataSet[,i])]   )
                        plot(dens, main=paste(col_names[i],"[,",i,"]",sep=""))
                        polygon(dens, col="#7F7FFF", border="black")
                        
                }
                else
                {
                        barplot(table(DataSet[,i]), border=NA, col = "#7F7FFF", main = paste(col_names[i],"[,",i,"]",sep=""))
                        if(class(DataSet[,i])!="factor")
                        {
                                mm <- abs( min(DataSet[,i]) )
                                turf <- mm + DataSet[,i]
                                L1 <- mean(turf) - ((3*sd(turf)) + mm)
                                L2 <- mean(turf) + (3*sd(turf)) - mm
                                #dens <- density(DataSet[,i])
                                #plot(dens, main=paste(col_names[i],"[,",i,"]",sep=""))
                                #polygon(dens, col="#4201FF", border="black")
                                abline(v = L1, lwd = 1, lty = 2)
                                abline(v = L2, lwd = 1, lty = 2)
                        }
                }
        }
}











## Density Plots
####For numeric & integer outliers<br>
# ```{r, echo=FALSE}
par(mfrow = c(2,3))
cols <- dim(DataSet)[2]
col_names <- colnames(DataSet)
for (i in 1:cols)
{
        if(  class(DataSet[,i]) %in% c("numeric","integer")  )
        {
                dens <- density(DataSet[,i])
                plot(dens, main=paste(col_names[i],"[",i,"]"))
                polygon(dens, col="#7F7FFF", border="black")
        }
}
# ```


if(  class(DataSet[,i]) %in% c("numeric","integer","factor")  )
{
        if(   sum( is.na(DataSet[,i]) )   >   0   )
        {
                print(col_names[i])
        }
        else
        {
                if(class(DataSet[,i])=="factor")
                {
                        barplot(table(DataSet[,i]), col = "#7F7FFF", main=col_names[i])
                }
                else
                {
                        dens <- density(DataSet[,i])
                        plot(dens, main=paste(col_names[i],"[",i,"]"))
                        polygon(dens, col="#7F7FFF", border="black")
                }
        }
}
for (i in 1:cols)
{
        if(class(DataSet[,i])=="factor")
        {
                if(   sum( is.na(DataSet[,i]) )   >   0   )
                {
                        print(col_names[i])
                }
                else
                {
                        barplot(table(DataSet[,i]), col = "#63028F", main=col_names[i])
                }
        }
        if(  class(DataSet[,i]) %in% c("numeric","integer","factor")  )
        {
                if(   sum( is.na(DataSet[,i]) )   >   0   )
                {
                        dens <- density(   DataSet[,i][!is.na(DataSet[,i])]   )
                        plot(dens, main=paste(col_names[i],"[,",i,"]",sep=""))
                        polygon(dens, col="#7F7FFF", border="black")
                        
                }
                else
                {
                        barplot(table(DataSet[,i]), border=NA, col = "#7F7FFF", main = paste(col_names[i],"[,",i,"]",sep=""))
                        
                        #L1 <- mean(DataSet[,i]) - (3*sd(DataSet[,i]))
                        #L2 <- mean(DataSet[,i]) +(3*sd(DataSet[,i]))
                        #dens <- density(DataSet[,i])
                        #plot(dens, main=paste(col_names[i],"[,",i,"]",sep=""))
                        #polygon(dens, col="#4201FF", border="black")
                        #abline(v = L1, lwd = 1, lty = 2)
                        #abline(v = L2, lwd = 1, lty = 2)
                }
        }
}




DataSet <- read.csv("exploratory/WearableComputing_weight_lifting_exercises_biceps_curl_variations.csv", na.strings = c("","NA"))
library(plotrix)
drops <- c("X", "user_name", "raw_timestamp_part_1", "raw_timestamp_part_2",
           "cvtd_timestamp", "new_window", "num_window")
remv_colms <- colnames(DataSet) %in% drops
DataSet <- DataSet[, !remv_colms]
NA_Col <- colSums( is.na(DataSet) )
NA_Colmns <- 100 * NA_Col / (dim(DataSet)[1])

NA_removal_Threshold_percent <- 80
NA_Colmns <- NA_Colmns > NA_removal_Threshold_percent
DataSet <- DataSet[, ! NA_Colmns]



DataSet[4,5] <- NA
NA_Colmns <- colSums( is.na(DataSet) )
NA_Colmns <- 100*NA_Colmns / (dim(DataSet)[1])
table(NA_Colmns[NA_Colmns > 0])
NA_Colmns[NA_Colmns > 0]















DataSet <- mtcars
DataSet[5,6] <- NA
DataSet[6,7] <- NA
DataSet[7,7] <- NA
#DataSet[4,5] <- NA

NA_Colmns <- colSums( is.na(DataSet) )
NA_Colmns <- 100 * NA_Colmns / (dim(DataSet)[1])
df_t <- NA_Colmns[NA_Colmns > 0]
NR <- length(df_t)
ny <- data.frame(  matrix(NA, ncol=1, nrow=1)  )
colnames(ny) <- c("NIL")
for(i in 1:NR)
{
        col_names <- colnames(ny)
        current_colm <- as.character(df_t[i])
        if(   current_colm %in% col_names   )
        {
                r <- sum(  !is.na(ny[,current_colm])  )+1
                ny[r,current_colm] <- names(df_t[i])
        }
        else
        {
                col_names <- c(col_names, df_t[i])
                ny[,length(col_names)] = NA
                ny[1,length(col_names)] <- names(df_t[i])
                colnames(ny) <- col_names
        }
}
ny
length(ny)
nrow(ny)
ncol(ny)

library(ggplot2)
hp<-qplot(x=DataSet[,5], fill=..count..) + scale_fill_gradient(low="blue", high="red")
hp
ggplot(diamonds, aes(price, fill = cut)) +
        geom_histogram(binwidth = 1)


tab <- table(DataSet[,5])
print(
        ggplot(   DataSet[rep(TRUE, times=length(tab)),], aes(  x=as.numeric( names(tab) )  )   ) +
                geom_abline(aes(x = as.numeric( names(tab) ),
                            y=0, xend=as.numeric( names(tab) ),
                            yend=tab))
)

ggplot() + 
        geom_point(data = tab, aes(as.numeric( names(tab) ), tab)) +
        geom_segment(aes(x = x, y = 0, xend = x, yend = y),
                     arrow = arrow(angle = 8,type = "closed",length = unit(0.10, "inches")), 
                     size = 0.2, 
                     linetype = 1,  
                     color = "#cccccc")


x <- college$rank
y <- college$median
library(ggplot2)
ggplot(data = college) +
        geom_segment(mapping = aes(x = x, y = 0, xend = x, yend = y)) +
        #geom_segment(aes(x = rank, y = median, xend = 0, yend = median)) +
        geom_point(mapping = aes(x = x, y = y))





plotMassSpectra <- function(X, na.rm = TRUE, ...) {
        
        nm <- names(X)
        
        for (i in seq_along(nm)) {
                
                plots <-ggplot(X,
                               aes_string(x = as.numeric( nm[i] ),
                                          xend = as.numeric( nm[i] ),
                                          y = 0,
                                          yend = nm[i])
                               ) + geom_segment(aes_string(color = "group")) +
                        
                        labs(x = "m/z", y = "Relative Abundance", title = paste("Reconstructed Mass Spectra", nm[i], "- QA1", sep = " "))
                
                #ggsave(plots, filename = paste("MS_", nm[i], ".jpg", sep=""))
                
        }
        print(plots)
}
plotMassSpectra(DataSet[,5])

ny <- ny[,2:ncol(ny)]

max(
table( DataSet[,28] )
)
barplot(
        table( DataSet[,28] ),
        col = "black"
)
legend("topleft", legend=head(table( DataSet[,28] )) )

rbind(head(table( DataSet[,28] )),tail(table( DataSet[,28] )))

points()
as.data.frame(table( DataSet[,27] ))$Var1
as.data.frame(table( NA_Colmns[NA_Colmns > 0] ))
sum(!is.na(DataSet[,27]))+1


x <- head(table( DataSet[,5] ))
y <- tail(table( DataSet[,5] ))

x <- dim(as.data.frame(table( DataSet[,5] )))
colnames(x)


n <- length(y)
rbind(
        colnames(y[1:head_tail_length]),
        y[1:head_tail_length],
        colnames(y[(n-head_tail_length):n]),
        y[(n-head_tail_length):n]
)

x <- table( DataSet[,5] )
names(x)


colnames(y[1:head_tail_length]),
y[1:head_tail_length],
colnames(y[(n-head_tail_length):n]),
y[(n-head_tail_length):n]







dens <- density(training$gyros_dumbbell_x)
plot(dens, main="gyros_dumbbell_x (-204)")
polygon(dens, col="#7F7FFF", border="black")


dens <- density(training$gyros_dumbbell_y)
plot(dens, main="gyros_dumbbell_y (52)")
polygon(dens, col="#7F7FFF", border="black")


dens <- density(training$gyros_dumbbell_z)
plot(dens, main="gyros_dumbbell_z (317)")
polygon(dens, col="#7F7FFF", border="black")


dens <- density(training$gyros_forearm_x)
plot(dens, main="gyros_forearm_x (-22)")
polygon(dens, col="#7F7FFF", border="black")


dens <- density(training$gyros_forearm_y)
plot(dens, main="gyros_forearm_y (311)")
polygon(dens, col="#7F7FFF", border="black")


dens <- density(training$gyros_forearm_z)
plot(dens, main="gyros_forearm_x (231)")
polygon(dens, col="#7F7FFF", border="black")


dens <- density(training$magnet_dumbbell_y)
plot(dens, main="magnet_dumbbell_y (-3600)")
polygon(dens, col="#7F7FFF", border="black")


library(ggplot2)
DataSet <- diamonds

DataSet[,1]
DataSet[,2]

View(DataSet)






















































NA_STRINGS <- c("", "NA", "?")
colm_types <- c("D","t","d","d","d","d","d","d","d")

ROWS_TO_READ <- 200000
file_remaining <- TRUE
SKIP_LINES <- 1
final <- readr::read_csv2("household_power_consumption.txt", n_max = 1,
                          na = NA_STRINGS, col_types = readr::cols(.default = "c"),
                          col_names = FALSE, quoted_na = FALSE, progress = FALSE )

while(file_remaining){
        initial <- readr::read_csv2( "household_power_consumption.txt", skip = SKIP_LINES,
                                     n_max = ROWS_TO_READ, na = NA_STRINGS,
                                     col_types = readr::cols(.default = "c"), col_names = FALSE,
                                     quoted_na = FALSE, progress = FALSE )
        
        if(   nrow(initial) < ROWS_TO_READ   ){
                file_remaining <- FALSE
        }
        
        initial <- initial[initial$X1 %in% c("1/2/2007","2/2/2007"),]
        final <- dplyr::bind_rows(final,initial)
        SKIP_LINES <- SKIP_LINES + ROWS_TO_READ
}

colnames(final) <- final[1,]
final <- final[-1,]

final <- readr::type_convert(final)
# View(final)











png(filename = "plot1.png", width=480, height=480)
hist(final$Global_active_power, col="red", main="Histogram of Global Active Power", xlab = "Global Active Power (kilowatts)")
dev.off()




x <- paste(final$Date,final$Time)

final$date_time <- strptime(x,format = "%d/%m/%Y %H:%M:%S")

png(filename = "plot2.png", width=480, height=480)
plot(final$date_time,final$Global_active_power,type = "l",xlab = "", ylab = "Global Active Power (kilowatts)")
dev.off()




x <- paste(final$Date,final$Time)
final$date_time <- strptime(x,format = "%d/%m/%Y %H:%M:%S")

png(filename = "plot3.png", width=480, height=480)
plot(final$date_time,final$Sub_metering_1,type = "l",xlab = "", ylab = "Energy sub metering")
lines(final$date_time,final$Sub_metering_2, col="red")
lines(final$date_time,final$Sub_metering_3, col="blue")
legend(x="topright",
       lty=1, lwd=2,
       legend = c("Sub_metering_1", "Sub_metering_2", "Sub_metering_3"),
       col=c("black", "red", "blue")
       )
dev.off()







png(filename = "plot4.png", width=480, height=480)

par(mfrow=c(2,2))
plot(final$date_time,final$Global_active_power,type = "l",xlab = "", ylab = "Global Active Power")

plot(final$date_time,final$Voltage,type = "l",xlab = "datetime", ylab = "Voltage")

plot(final$date_time,final$Sub_metering_1,type = "l",xlab = "", ylab = "Energy sub metering")
lines(final$date_time,final$Sub_metering_2, col="red")
lines(final$date_time,final$Sub_metering_3, col="blue")
legend(x="topright",
       lty=1, lwd=2,
       legend = c("Sub_metering_1", "Sub_metering_2", "Sub_metering_3"),
       col=c("black", "red", "blue"),
       bty = "n"
)

plot(final$date_time,final$Global_reactive_power,type = "l",xlab = "datetime", ylab = "Global_reactive_power")

dev.off()
































legend("topright", lty=1, lwd =3, col=c("black","red","blue") ,legend=c("Sub_metering_1","Sub_metering_2","Sub_metering_3"))


legend(1, 95, legend=c("Line 1", "Line 2"),
       col=c("red", "blue"), lty=1:2, cex=0.8)






x<-1:10; y1=x*x; y2=2*y1
plot(x, y1, type="b", pch=19, col="red", xlab="x", ylab="y")
# Add a line
lines(x, y2, pch=18, col="blue", type="b", lty=2)
# Add a legend
legend(1, 95, legend=c("Line 1", "Line 2"),
       col=c("red", "blue"), lty=1:2, cex=0.8)
makePlot<-function(){
        x<-1:10; y1=x*x; y2=2*y1
        plot(x, y1, type="b", pch=19, col="red", xlab="x", ylab="y")
        lines(x, y2, pch=18, col="blue", type="b", lty=2)
}
makePlot()
# Add a legend to the plot
legend(1, 95, legend=c("Line 1", "Line 2"),
       col=c("red", "blue"), lty=1:2, cex=0.8,
       title="Line types", text.font=4, bg='lightblue')



as.POSIXct(paste(final$Date, as.Date(final$Date, format = "%d/%m/%Y")))

plot(final$Global_active_power~final$Date, type="l",
     col="red",
     main="Histogram of Global Active Power",
     xlab = "Global Active Power (kilowatts)")



sum(is.na(final))



final$x <- strptime(c("2006-01-08 10:07:52", "2006-08-07 19:33:02"),
              "%Y-%m-%d %H:%M:%S", tz = "EST5EDT")

as.POSIXct()
> a <- "2016-12-05-16.25.54.875000"
> as.POSIXct(a, format="%Y-%m-%d-%H.%M.%S")


grep
###############################################################################



View(SCC)

unique(SCC$EI.Sector)
unique(SCC$Option.Group)
unique(SCC$Option.Set)
unique(SCC$SCC.Level.One)
unique(SCC$SCC.Level.Two)
unique(SCC$SCC.Level.Three)
unique(SCC$SCC.Level.Four)
unique(SCC$Map.To)
unique(SCC$Last.Inventory.Year)

summ






###############################################################################

NEI <- readRDS("summarySCC_PM25.rds")
SCC <- readRDS("Source_Classification_Code.rds")

head()
summary(  NEI$year  )
summary(  NEI$Emissions  )
summary(  NEI$fips  )
summary(  SCC$EI  )
summary(  SCC$SCC  )

NEI$year[100] <- ""
str(  NEI$year  )
str(  NEI$Emissions  )
str(  NEI$fips  )
str(  SCC$EI  )
str(  SCC$SCC  )

str(SCC)

sum(is.na(  SCC$Created_Date  ))

png(filename = "plot1.png", width=600, height=600)
par(bg="#FFFFFF")
total_emissions <- dplyr::summarise(  dplyr::group_by(NEI, year), Emissions=sum(Emissions)  )
plot <- barplot(height = total_emissions$Emissions/1000, # dividing by 1000 converts y-axis to kilo-tons
                xlab="Time in years", ylab=expression('Total PM'[2.5]*' emissions in Kilo-tons'),
                ylim=c(0,8100),
                names.arg = total_emissions$year,
                main=expression('Total PM'[2.5]*' emissions across United States in Kilo-tons'),col=c("#800080","#D896FF","#BE29EC","#660066"))

## Adding values above the bars
text(x = plot,
     y = round(total_emissions$Emissions/1000,6),
     label = round(total_emissions$Emissions/1000,3),
     pos = 3, cex = 1, col = "black", font=2)
dev.off()








png(filename = "plot2.png", width=600, height=600)
# PLOT
par(bg="#FFFFFF")
baltimore <- NEI[NEI$fips=="24510",]
total_emissions <- dplyr::summarise(  dplyr::group_by(baltimore, year), Emissions=sum(Emissions)  )
plot <- barplot(height = total_emissions$Emissions,
                xlab="Time in years", ylab=expression('Total PM'[2.5]*' emissions of Baltimore City in tons'),
                ylim=c(0,3800),
                names.arg = total_emissions$year,
                main=expression('Total PM'[2.5]*' emissions of Baltimore City in tons'),col=c("#C22326","#FDB632","#F37338","#801638"))

## Adding values above the bars
text(x = plot,
     y = round(total_emissions$Emissions,6),
     label = round(total_emissions$Emissions,3),
     pos = 3, cex = 1, col = "black", font=2)
dev.off()









png(filename = "plot3.png", width=978, height=550)
# PLOT
library(ggplot2)
baltimore <- NEI[NEI$fips=="24510",]
total_emissions <- dplyr::summarise(  dplyr::group_by(baltimore, type, year), Emissions=sum(Emissions)  )
ggplot(data=total_emissions, mapping = aes(x=factor(year),y=Emissions,fill=type)) +
        geom_bar(stat="identity") + 
        geom_label(aes(label=round(Emissions,2)), colour = "black", fontface = "bold", fill="white")+
        facet_grid(.~type)+
        xlab("Years")+
        ylab(expression('Baltimore City PM'[2.5]*' emissions in tons'))+
        ggtitle(label=expression('Baltimore City PM'[2.5]*' emissions, faceted by emission type'))+
        theme(plot.title = element_text(hjust = 0.5),legend.position="none")
dev.off()








sources <- SCC$EI.Sector %in% c(
        "Fuel Comb - Electric Generation - Coal",
        "Fuel Comb - Comm/Institutional - Coal",
        "Fuel Comb - Industrial Boilers, ICEs - Coal")

coal <- NEI$SCC %in% SCC[sources,]$SCC
nei_coal <- NEI[coal,]

total_emissions <- dplyr::summarise(  dplyr::group_by(nei_coal, year), Emissions=sum(Emissions)/1000  )

png(filename = "plot4.png", width=600, height=600)
ggplot(data=total_emissions, mapping = aes(x=factor(year),y=Emissions,fill=year))+
        geom_bar(stat="identity")+
        geom_label(aes(label=round(Emissions,2)), colour = "black", fontface = "bold", fill="white")+
        xlab("Time in years")+
        ylab(expression('Total PM'[2.5]*' emissions in kilo-tons'))+
        ggtitle(label=expression('Emissions of PM'[2.5]*' across United States from coal combustion-related sources'))+
        theme(plot.title = element_text(hjust = 0.5),legend.position="none")
dev.off()





sources <- c(
        "Mobile - On-Road Gasoline Light Duty Vehicles",
        "Mobile - On-Road Gasoline Heavy Duty Vehicles",
        "Mobile - On-Road Diesel Light Duty Vehicles",
        "Mobile - On-Road Diesel Heavy Duty Vehicles")

scc2 <- SCC[ SCC$EI.Sector %in% sources, ]
temp <- NEI[ NEI$SCC %in% scc2$SCC, ]
baltimore <- temp[ temp$fips %in% "24510", ]

total_emissions <- dplyr::summarise( dplyr::group_by(baltimore, year), Emissions=sum(Emissions))

png(filename = "plot5.png", width=600, height=600)
ggplot(data=total_emissions, mapping = aes(x=factor(year), y=Emissions))+
        geom_bar(stat="identity",fill=c("#F9766E","#00BEC6","#00BEC6","#F9766E"))+
        geom_label(mapping = aes(x=factor(year),y=Emissions,label=round(Emissions,2)))+
        xlab("Years")+
        ylab(expression('Baltimore City, Maryland emissions in tons'))+
        ggtitle(label=expression('PM'[2.5]*' emissions from motor vehicle sources in Baltimore City, Maryland'))+
        theme(plot.title = element_text(hjust = 0.5))
dev.off()






scc2 <- SCC[ SCC$EI.Sector %in% sources, ]
temp <- NEI[ NEI$SCC %in% scc2$SCC, ]
bal_los <- temp[ temp$fips %in% c("24510","06037"), ]
bal_los$fips <- as.factor(bal_los$fips)
levels(bal_los$fips) <- c("Los Angeles County, California","Baltimore City, Maryland")

# PLOT
png(filename = "plot6.png", width=978, height=550)
total_emissions <- dplyr::summarise( dplyr::group_by(bal_los, fips, year), Emissions=sum(Emissions) )
ggplot(data=total_emissions, mapping = aes(x=factor(year),y=Emissions,fill=fips))+
        geom_bar(stat="identity")+
        geom_label(aes(label=round(Emissions,2)), colour = "black", fontface = "bold", fill="white")+
        facet_grid(.~fips)+
        xlab("Years")+
        ylab(expression('PM'[2.5]*' emissions in tons'))+
        ggtitle(label=expression('Los Angeles & Baltimore City\'s PM'[2.5]*' emissions in tons'))+
        theme(plot.title = element_text(hjust = 0.5),legend.position="none")
dev.off()


#################################################






library("data.table") path <- getwd()
download.file(url = "https://d396qusza40orc.cloudfront.net/exdata%2Fdata%2FNEI_data.zip" , destfile = paste(path, "dataFiles.zip", sep = "/"))
unzip(zipfile = "dataFiles.zip")
SCC <- data.table::as.data.table(x = readRDS(file = "Source_Classification_Code.rds"))
NEI <- data.table::as.data.table(x = readRDS(file = "summarySCC_PM25.rds")) # Prevents histogram from printing in scientific notation
NEI[, Emissions := lapply(.SD, as.numeric), .SDcols = c("Emissions")]
totalNEI <- NEI[, lapply(.SD, sum, na.rm = TRUE), .SDcols = c("Emissions"), by = year]
barplot(totalNEI[, Emissions] , names = totalNEI[, year] , xlab = "Years", ylab = "Emissions" , main = "Emissions over the Years")




library("data.table")
path <- getwd()
download.file(url = "https://d396qusza40orc.cloudfront.net/exdata%2Fdata%2FNEI_data.zip" , destfile = paste(path, "dataFiles.zip", sep = "/"))
unzip(zipfile = "dataFiles.zip")
SCC <- data.table::as.data.table(x = readRDS(file = "Source_Classification_Code.rds"))
NEI <- data.table::as.data.table(x = readRDS(file = "summarySCC_PM25.rds"))
NEI[, Emissions := lapply(.SD, as.numeric), .SDcols = c("Emissions")]
totalNEI <- NEI[fips=='24510', lapply(.SD, sum, na.rm = TRUE) , .SDcols = c("Emissions") , by = year]
png(filename='plot2.png')
barplot(totalNEI[, Emissions] , names = totalNEI[, year] , xlab = "Years", ylab = "Emissions" , main = "Emissions over the Years")
dev.off()




library(plyr)
library(ggplot2)
library(data.table)
library(grid)
library(scales)
library(httr)
setwd("~/Exploratory_Data_Analysis/Project 02")
if (!file.exists("./data/NEI_data")) { unzip("./data/NEI_data.zip", exdir = "./data/NEI_data") }
if (!"NEI" %in% ls()) { NEI <- readRDS("./data/NEI_data/summarySCC_PM25.rds") }
if (!"SCC" %in% ls()) { SCC <- readRDS("./data/NEI_data/Source_Classification_Code.rds") }
if (!"BaltimoreData" %in% ls()) { BaltimoreData <- NEI[NEI$fips == "24510", ] }
if (!"BData" %in% ls()) { BData <- ddply(BaltimoreData, .(type, year), summarize, TotalEmissions = sum(Emissions)) }
png(filename = "Plot3.png", width = 500, height = 480, units = "px")
ggplot(BData, aes(year, TotalEmissions, colour = type)) +
        geom_line() + geom_point() +
        labs(title = expression('Total PM'[2.5]*" Emissions in Baltimore City, Maryland from 1999 to 2008"), x = "Year", y = expression('Total PM'[2.5]*" Emission (in tons)"))
dev.off()




if(!exists("NEI")){ NEI <- readRDS("./data/summarySCC_PM25.rds") }
if(!exists("SCC")){ SCC <- readRDS("./data/Source_Classification_Code.rds") }
# merge the two data sets
if(!exists("NEISCC")){ NEISCC <- merge(NEI, SCC, by="SCC") }
library(ggplot2)
coalMatches <- grepl("coal", NEISCC$Short.Name, ignore.case=TRUE)
subsetNEISCC <- NEISCC[coalMatches, ]
aggregatedTotalByYear <- aggregate(Emissions ~ year, subsetNEISCC, sum)
png("plot4.png", width=640, height=480)
g <- ggplot(aggregatedTotalByYear, aes(factor(year), Emissions))
g <- g +
        geom_bar(stat="identity") +
        xlab("year") +
        ylab(expression('Total PM'[2.5]*" Emissions")) +
        ggtitle('Total Emissions from coal sources from 1999 to 2008')
print(g)
dev.off()




if(!exists("NEI")){ NEI <- readRDS("./data/summarySCC_PM25.rds") } if(!exists("SCC")){ SCC <- readRDS("./data/Source_Classification_Code.rds") } # merge the two data sets if(!exists("NEISCC")){ NEISCC <- merge(NEI, SCC, by="SCC") } library(ggplot2) subsetNEI <- NEI[NEI$fips=="24510" & NEI$type=="ON-ROAD", ] aggregatedTotalByYear <- aggregate(Emissions ~ year, subsetNEI, sum) png("plot5.png", width=840, height=480) g <- ggplot(aggregatedTotalByYear, aes(factor(year), Emissions)) g <- g + geom_bar(stat="identity") + xlab("year") + ylab(expression('Total PM'[2.5]*" Emissions")) + ggtitle('Total Emissions from motor vehicle (type = ON-ROAD) in Baltimore City, Maryland (fips = "24510") from 1999 to 2008') print(g) dev.off()



if(!exists("NEI")){ NEI <- readRDS("./data/summarySCC_PM25.rds") } if(!exists("SCC")){ SCC <- readRDS("./data/Source_Classification_Code.rds") } # merge the two data sets if(!exists("NEISCC")){ NEISCC <- merge(NEI, SCC, by="SCC") } library(ggplot2) # Compare emissions from motor vehicle sources in Baltimore City with emissions from motor fips == "06037"). subsetNEI <- NEI[(NEI$fips=="24510"|NEI$fips=="06037") & NEI$type=="ON-ROAD", ] aggregatedTotalByYearAndFips <- aggregate(Emissions ~ year + fips, subsetNEI, sum) aggregatedTotalByYearAndFips$fips[aggregatedTotalByYearAndFips$fips=="24510"] <- "Baltimore, MD" aggregatedTotalByYearAndFips$fips[aggregatedTotalByYearAndFips$fips=="06037"] <- "Los Angeles, CA" png("plot6.png", width=1040, height=480) g <- ggplot(aggregatedTotalByYearAndFips, aes(factor(year), Emissions)) g <- g + facet_grid(. ~ fips) g <- g + geom_bar(stat="identity") + xlab("year") + ylab(expression('Total PM'[2.5]*" Emissions")) + ggtitle('Total Emissions from motor vehicle (type=ON-ROAD) in Baltimore City, MD (fips = "24510") vs Los Angeles, CA (fips = "06037") 1999-2008') print(g) dev.off()
```
