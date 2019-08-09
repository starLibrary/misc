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

library(ggplot2)
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
                    dashboardHeader(title = "Analyzer2"),
                    dashboardSidebar( 
                            sidebarMenu(
                                    menuItem("Preliminaries", tabName = "sumhead", icon = icon("dashboard")),
                                    menuItem("Bar Plots", tabName = "barplots", icon = icon("th")),
                                    menuItem("X-Y Scatter Plots", tabName = "xyscatplots", icon = icon("archway")),
                                    selectInput("rawdata", "Choose a dataset", size = 23, selectize = FALSE, choices = A),
                                    menuItem("Shiny Tutorial", icon = icon("archway"),
                                             menuSubItem("dashboard template", tabName = "dashtem"),
                                             menuSubItem("example-1", tabName = "example-1"),
                                             menuSubItem("example-2", tabName = "example-2")
                                    )
                            )
                    ),
                    dashboardBody(h3(textOutput("description")),
                                  h5("data class"),
                                  verbatimTextOutput("class"),
                                  h5("data columns"),
                                  verbatimTextOutput("colmnames"),
                                  verbatimTextOutput("colmnames2"),
                                  
                                  
                                  tabItems(
                                          tabItem(tabName = "sumhead",
                                                  
                                                  h3("NA data statistics"),
                                                  h4("% of NAs per column (column headers are in %s) (column indices in [ ])"),
                                                  h4("Remove/Impute any NAs before proceeding further."),
                                                  verbatimTextOutput("NAs"),
                                                  h4("Data Summaries"),
                                                  verbatimTextOutput("str"),
                                                  verbatimTextOutput("summary"),
                                                  h4("Full Data"),
                                                  verbatimTextOutput("fulldata")
                                          ),
                                          tabItem(tabName = "barplots",
                                                  selectInput("column", "Choose a column", choices = character(0)),
                                                  verbatimTextOutput("colmclass"),
                                                  verbatimTextOutput("colmstr"),
                                                  actionButton(inputId = "barplot", label = "Build bar-plots"),
                                                  plotOutput("barplots")
                                          ),
                                          tabItem(tabName = "xyscatplots"),
                                          tabItem(tabName = "dashtem")
                                  )
                                  
                                  
                    )
)

server <- function(input, output, session) {
        
        
        
        observeEvent(input$rawdata,{
                
                output$description <- renderText({
                        Z[match(input$rawdata,X)]
                })
                Dataset <- get0(input$rawdata)
                
                if(  is.null(Dataset)  ){
                        
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
                        output$barplots <- renderPlot({  NULL  })
                }else{
                        output$class <- renderPrint({  class(Dataset)  })
                        output$str <- renderPrint({  str(Dataset)  })
                        output$summary <- renderPrint({  summary(Dataset)  })
                        output$fulldata <- renderPrint({  Dataset  })
                        
                        colnems <- colnames(Dataset)
                        if((is.null(colnems)) || (length(colnems)==1)){
                                output$NAs <- renderPrint({
                                        paste0(100*sum( is.na(Dataset) )/( length(Dataset) )," %")
                                })
                                output$colmnames <- renderPrint({  colnems  })
                                output$colmnames2 <- renderPrint({  names(Dataset)  })
                                updateSelectInput(session, "column",  choices = input$rawdata)
                                output$colmclass <- renderPrint({  class(Dataset)  })
                                output$colmstr <- renderPrint({  str(Dataset)  })
                                
                                
                                output$barplots <- renderPlot({
                                        Freq <- table(Dataset)
                                        percent <- as.numeric(   as.character( Freq*100/length(Dataset) )   )
                                        Freq2 <- as.data.frame(cbind(Freq,percent))
                                        if(is.numeric(Dataset)){  Freq2$name2 <- as.numeric(names(Freq))  }
                                        if(is.factor(Dataset)){  Freq2$name2 <- names(Freq)  }
                                        rownames(Freq2) <- NULL
                                        ggplot(data = Freq2, mapping = aes(x=name2, y=Freq)) +
                                                geom_bar(stat="identity", fill="black") +
                                                xlab(input$column) +
                                                ylab("frequency")
                                })
                        }else{
                                #Dataset <- as.data.frame(Dataset)
                                output$NAs <- renderPrint({
                                        NAS <- 100*colSums(is.na(Dataset))/(dim(Dataset)[1])
                                        NA_list <- tapply(NAS, as.factor(NAS), function(x) paste0(names(x),"[",match(names(x),colnems),"]"), simplify=FALSE)
                                        #NA_list <- tapply(NAS, as.factor(NAS), names, simplify = FALSE)
                                        max_is <- max(vapply(NA_list, length, numeric(1)))
                                        #as.data.frame(sapply(    NA_list,    function(x)    c(   x,   rep("",max_is-length(x))   )    ))
                                        as.data.frame(vapply(NA_list, function(x) c(x, rep("",max_is-length(x))), character(max_is)))
                                })
                                output$colmnames <- renderPrint({  colnems  })
                                output$colmnames2 <- renderPrint({  names(Dataset)  })
                                updateSelectInput(session, "column",  choices = colnems)
                                
                                if(is.table(Dataset)||is.array(Dataset)||is.matrix(Dataset)){
                                        output$colmclass <- renderPrint({  class(Dataset[,input$column])  })
                                        output$colmstr <- renderPrint({  str(Dataset[,input$column])  })
                                        output$barplots <- renderPlot({
                                                Colmn <- Dataset[,input$column]
                                                Freq <- table(Colmn)
                                                percent <- as.numeric(   as.character( Freq*100/dim(Dataset)[1] )   )
                                                Freq2 <- as.data.frame(cbind(Freq,percent))
                                                if(is.numeric(Colmn)){  Freq2$name2 <- as.numeric(names(Freq))  }
                                                if(is.factor(Colmn)){  Freq2$name2 <- names(Freq)  }
                                                rownames(Freq2) <- NULL
                                                ggplot(data = Freq2, mapping = aes(x=name2, y=Freq)) +
                                                        geom_bar(stat="identity", fill="black") +
                                                        xlab(input$column) +
                                                        ylab("frequency")
                                        })
                                }else{
                                        output$colmclass <- renderPrint({  class(Dataset[[input$column]])  })
                                        output$colmstr <- renderPrint({  str(Dataset[[input$column]])  })
                                        
                                        output$barplots <- renderPlot({
                                                Colmn <- Dataset[[input$column]]
                                                Freq <- table(Colmn)
                                                percent <- as.numeric(   as.character( Freq*100/dim(Dataset)[1] )   )
                                                Freq2 <- as.data.frame(cbind(Freq,percent))
                                                if(is.numeric(Colmn)){  Freq2$name2 <- as.numeric(names(Freq))  }
                                                if(is.factor(Colmn)){  Freq2$name2 <- names(Freq)  }
                                                rownames(Freq2) <- NULL
                                                ggplot(data = Freq2, mapping = aes(x=name2, y=Freq)) +
                                                        geom_bar(stat="identity", fill="black") +
                                                        xlab(input$column) +
                                                        ylab("frequency %")
                                        })
                                }
                        }
                }
        })
}

shinyApp(ui, server)
#str(Dataset[[input$column]])







```
