```R
#
# This is a Shiny web application. You can run the application by clicking
# the 'Run App' button above.
#
# Find out more about building applications with Shiny here:
#
#    http://shiny.rstudio.com/
#


library(shiny)
library(shinydashboard)

library(ISLR)
library(gapminder)
library(mvoutlier)
library(AER)
library(ggplot2)

library(data.table)

W <- data()[["results"]]
X <- W[,3]
Z <- paste0(X," : ",W[,4])
A <- sort(X)

NA_Data_3 <- function( Dataset ){
        if(is.null(Dataset)){
                return(NULL)
        }
        colnems <- colnames(Dataset)
        if((is.null(colnems)) || (length(colnems)==1)){
                return(     paste0(100*sum( is.na(Dataset) )/( length(Dataset) )," %")     )
        }
        NAS <- 100*colSums(is.na(Dataset))/(dim(Dataset)[1])
        NA_list <- tapply(NAS, as.factor(NAS), function(x) paste0(names(x),"[",match(names(x),colnems),"]"), simplify=FALSE)
        #NA_list <- tapply(NAS, as.factor(NAS), names, simplify = FALSE)
        max_is <- max(vapply(NA_list, length, numeric(1)))
        #as.data.frame(sapply(    NA_list,    function(x)    c(   x,   rep("",max_is-length(x))   )    ))
        return(as.data.frame(vapply(NA_list, function(x) c(x, rep("",max_is-length(x))), character(max_is))))
}

ui <- dashboardPage(skin = "green",
                    dashboardHeader(title = "Analyzer"),
                    dashboardSidebar( 
                            sidebarMenu(
                                    menuItem("Data Structure", tabName = "sumhead", icon = icon("dashboard")),
                                    menuItem("NA stats", tabName = "nastats", icon = icon("th")),
                                    menuItem("Distribution Plots", tabName = "barplots", icon = icon("th")),
                                    menuItem("X-Y Scatter Plots", tabName = "xyscatplots", icon = icon("archway")),
                                    menuItem("Log msgs", tabName = "logs", icon = icon("archway")),
                                    selectInput("rawdata", "Choose a dataset", size = 23, selectize = FALSE, choices = A)
                            )
                    ),
                    dashboardBody(h3(textOutput("description")),
                                  
                                  tabItems(
                                          tabItem(tabName = "sumhead",
                                                  h4("data class"),
                                                  verbatimTextOutput("class"),
                                                  h4("Data Summaries"),
                                                  verbatimTextOutput("summary"),
                                                  verbatimTextOutput("str"),
                                                  h4("Full Data"),
                                                  verbatimTextOutput("fulldata")
                                          ),
                                          tabItem(tabName = "nastats",
                                                  h3("NA data statistics"),
                                                  h4("% of NAs per column (column headers are in %s) (column indices in [ ])"),
                                                  h4("Remove/Impute any NAs before proceeding further."),
                                                  verbatimTextOutput("NAs")
                                          ),
                                          tabItem(tabName = "barplots",
                                                  h4("data columns"),
                                                  verbatimTextOutput("colmnames"),
                                                  verbatimTextOutput("colmnames2"),
                                                  selectInput("column", "Choose a column", choices = character(0)),
                                                  h4("columns class and structure"),
                                                  verbatimTextOutput("colmclass"),
                                                  actionButton(inputId = "barplot", label = "Build bar-plots"),
                                                  plotOutput("freqplots")
                                          ),
                                          tabItem(tabName = "xyscatplots",
                                                  selectInput("coltwo", "Choose Y-axis column", choices = character(0)),
                                                  selectInput("colone", "Choose X-axis column", choices = character(0)),
                                                  plotOutput("scatplot")
                                          ),
                                          tabItem(tabName = "logs",
                                                  h4("data class / if condition"),
                                                  verbatimTextOutput("type"),
                                                  h4("columns class and structure"),
                                                  verbatimTextOutput("colmstr")
                                          )
                                  )
                    )
)

server <- function(input, output, session) {
        
        
        
        observeEvent(input$rawdata,{
                
                output$description <- renderText({
                        Z[match(input$rawdata,X)]
                })
                #Dataset <- as.data.table( get0(input$rawdata) )
                Dataset <- get0(input$rawdata)
                
                output$class <- renderPrint({  class(Dataset)  })
                output$str <- renderPrint({  str(Dataset)  })
                output$summary <- renderPrint({  summary(Dataset)  })
                output$colmnames <- renderPrint({  colnames(Dataset)  })
                output$colmnames2 <- renderPrint({  names(Dataset)  })
                
                output$fulldata <- renderPrint({  Dataset  })
                
                if(  is.null(Dataset)  ){
                        output$type <- renderPrint({  NULL  })
                        
                        output$class <- renderPrint({  NULL  })
                        output$str <- renderPrint({  NULL  })
                        output$summary <- renderPrint({  NULL  })
                        output$NAs <- renderPrint({  NULL  })
                        output$fulldata <- renderPrint({  NULL  })
                        output$colmnames <- renderPrint({  NULL  })
                        output$colmnames2 <- renderPrint({  NULL  })
                        updateSelectInput(session, "column",  choices = character(0))
                        output$colmclass <- renderPrint({  NULL  })
                        output$colmstr <- renderPrint({  NULL  })
                        output$freqplots <- renderPlot({  NULL  })
                        updateSelectInput(session, "colone",  choices = character(0))
                        updateSelectInput(session, "coltwo",  choices = character(0))
                        output$scatplot <- renderPlot({  NULL  })
                }else{
                        if(is.data.frame(Dataset)){
                                output$type <- renderPrint({  "DATA-FRAME"  })
                                colnems <- colnames(Dataset)
                                output$NAs <- renderPrint({
                                        NAS <- 100*colSums(is.na(Dataset))/(dim(Dataset)[1])
                                        NA_list <- tapply(NAS, as.factor(NAS), function(x) paste0(names(x),"[",match(names(x),colnems),"]"), simplify=FALSE)
                                        #NA_list <- tapply(NAS, as.factor(NAS), names, simplify = FALSE)
                                        max_is <- max(vapply(NA_list, length, numeric(1)))
                                        #as.data.frame(sapply(    NA_list,    function(x)    c(   x,   rep("",max_is-length(x))   )    ))
                                        as.data.frame(vapply(NA_list, function(x) c(x, rep("",max_is-length(x))), character(max_is)))
                                })
                                updateSelectInput(session, "column",  choices = colnems)
                                output$colmclass <- renderPrint({  class(Dataset[[input$column]])  })
                                output$colmstr <- renderPrint({  str(Dataset[[input$column]])  })
                                
                                output$freqplots <- renderPlot({
                                        Colmn <- Dataset[[input$column]]
                                        Freq2 <- as.data.frame(table(Colmn))
                                        if(is.numeric(Colmn)){  Freq2[,1] <- as.numeric(as.character(Freq2[,1]))  }
                                        if(is.factor(Colmn)){  Freq2[,1] <- as.character(Freq2[,1])  }
                                        ggplot(data = Freq2) +
                                                geom_segment(mapping = aes(x = Freq2[["Colmn"]], y = 0, xend = Freq2[["Colmn"]], yend = Freq2[["Freq"]])) +
                                                xlab(input$column) + ylab("frequency")
                                })
                                updateSelectInput(session, "colone",  choices = colnems)
                                updateSelectInput(session, "coltwo",  choices = colnems)
                                output$scatplot <- renderPlot({
                                        ggplot(data = Dataset, mapping=aes(x=Dataset[[input$colone]], y=Dataset[[input$coltwo]])) +
                                                #geom_jitter(color="salmon") +
                                                geom_point(size=2) +
                                                xlab(input$colone) + ylab(input$coltwo)
                                })
                        }else{
                                if(is.list(Dataset)){
                                        output$type <- renderPrint({  "LIST"  })
                                        output$NAs <- renderPrint({  NULL  })
                                        updateSelectInput(session, "column",  choices = character(0))
                                        output$colmclass <- renderPrint({  NULL  })
                                        output$colmstr <- renderPrint({  NULL  })
                                        output$freqplots <- renderPlot({  NULL  })
                                        updateSelectInput(session, "colone",  choices = character(0))
                                        updateSelectInput(session, "coltwo",  choices = character(0))
                                        output$scatplot <- renderPlot({  NULL  })
                                }else{
                                        output$type <- renderPrint({  "non-LIST"  })
                                        output$NAs <- renderPrint({  NULL  })
                                        updateSelectInput(session, "column",  choices = character(0))
                                        output$colmclass <- renderPrint({  NULL  })
                                        output$colmstr <- renderPrint({  NULL  })
                                        output$freqplots <- renderPlot({  NULL  })
                                        updateSelectInput(session, "colone",  choices = character(0))
                                        updateSelectInput(session, "coltwo",  choices = character(0))
                                        output$scatplot <- renderPlot({  NULL  })
                                }
                        }
                }
        })
}

shinyApp(ui, server)
#str(Dataset[[input$column]])


```
