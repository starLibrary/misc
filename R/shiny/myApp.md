### ui.R
```R
#
# This is the user-interface definition of a Shiny web application. You can
# run the application by clicking 'Run App' above.
#
# Find out more about building applications with Shiny here:
# 
#    http://shiny.rstudio.com/
#

library(shiny)
A <- c(  sort(unique(ggplot2::map_data("world")$region))  )

# Define UI for application that draws a histogram
shinyUI(fluidPage(
        titlePanel("The World..!!"),
        sidebarLayout(position = "right",
                      sidebarPanel(
                              selectInput("country", "Choose a country", size = 25, selectize = FALSE,
                                          choices = A
                              ),
                              
                              numericInput("obs", "Observations:", 10)
                      ),
                      mainPanel=mainPanel(
                              #textOutput("hvrhvrhvr"),
                              plotOutput("plot1", hover=hoverOpts(id="plot_hover_1")),
                              plotOutput("plot2")
                      )
        )
))

```

### server.R
```R
#
# This is the server logic of a Shiny web application. You can run the 
# application by clicking 'Run App' above.
#
# Find out more about building applications with Shiny here:
# 
#    http://shiny.rstudio.com/
#

library(shiny)

# Define server logic required to draw a histogram
shinyServer(function(input, output) {
        library(ggplot2)
        world <- map_data("world")
        countries <- unique(world$region)
        G <- ggplot()+
                geom_polygon(data=world, aes(x=long, y=lat, group=group, fill=group), colour = "black")+
                theme(legend.position = "none")+
                labs(x=NULL, y=NULL)+
                #scale_x_continuous(labels = NULL, breaks = NULL) +
                #scale_y_continuous(labels = NULL, breaks = NULL) +
                #coord_quickmap()+
                theme(panel.background = element_rect(fill = "#2998ed"))
        
        country <- reactive({input$country})
        
        
        output$plot1 = renderPlot({
                mapdf <- world[world$region %in% country(),]
                #tiff(filename = "world.tiff", width=600, height=600)
                G+geom_polygon(data=mapdf, aes(x=long, y=lat, group=group), fill="red", colour = "black")
        })
        
        
        output$plot2 = renderPlot({
                mapdf <- world[world$region %in% country(),]
                ggplot()+
                        geom_polygon(data=mapdf, aes(x=long, y=lat, group=group, fill=group), colour = "black")+
                        theme(legend.position = "none")+
                        labs(x=NULL, y=NULL)+
                        #scale_x_continuous(labels = NULL, breaks = NULL) +
                        #scale_y_continuous(labels = NULL, breaks = NULL) +
                        theme(panel.background = element_rect(fill = "#2998ed"))+
                        coord_quickmap()
        })
        
        hoverdata <- reactive({
                hvdt <- hoverOpts(input$plot_hover_1)
                if(hvdt){
                        return(NULL)
                }
                hvdt
        })
        
        output$hvrhvrhvr <- renderPrint({
                hoverdata$x
        })
        
})

```
