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

library(ISLR)
library(gapminder)
library(mvoutlier)
library(AER)
library(ggplot2)


W <- data()[["results"]]
X <- W[,3]
Z <- paste0(X," : ",W[,4])
A <- sort(X)


NA_Data_3 <- function( Dataset ){
        # if(is.null(Dataset)){  return(NULL)  }
        colnems <- colnames(Dataset)
        #if((is.null(colnems)) || (length(colnems)==1)){  return(     paste0(100*sum( is.na(Dataset) )/( length(Dataset) )," %")     )  }
        NAS <- 100*colSums(is.na(Dataset))/(dim(Dataset)[1])
        NA_list <- tapply(NAS, as.factor(NAS), function(x) paste0(names(x),"[",match(names(x),colnems),"]"), simplify=FALSE)
        #NA_list <- tapply(NAS, as.factor(NAS), names, simplify = FALSE)
        max_is <- max(vapply(NA_list, length, numeric(1)))
        #as.data.frame(sapply(    NA_list,    function(x)    c(   x,   rep("",max_is-length(x))   )    ))
        return(as.data.frame(vapply(NA_list, function(x) c(x, rep("",max_is-length(x))), character(max_is))))
}

Distribution <- function(Colmn, Colmn_name){
        
        Freq2 <- as.data.frame(table(Colmn))
        if(is.numeric(Colmn)){# cannot plot if NAs are present
                Freq2[,1] <- as.numeric(as.character(Freq2[,1]))
                max_freq <- max(Freq2$Freq)
                
                qn <- quantile(Colmn, probs=c(0.25,0.5,0.75))
                IQR <- qn[3] - qn[1]
                
                S <- min(Freq2[["Colmn"]])
                x_min <- qn[1] - (1.5*IQR)
                x_min <- ifelse( S<x_min, x_min, S )
                
                x_mid <- qn[2]
                
                S <- max(Freq2[["Colmn"]])
                x_max <- qn[3] + (1.5*IQR)
                x_max <- ifelse( S>x_max, x_max, S )
                
                Y_MIN <- max_freq*1.1
                Y_MAX <- Y_MIN+(max_freq*0.1)
                
                G <- ggplot() +
                        geom_segment(data=Freq2, mapping = aes(x=Freq2[["Colmn"]], y=0, xend=Freq2[["Colmn"]], yend=Freq2[["Freq"]])) +
                        #geom_point(data=Freq2, mapping=aes(x=Colmn, y=Freq)) +
                        xlab(Colmn_name) + ylab("frequency") +
                        geom_segment(mapping=aes(  x=x_min, y=Y_MIN+(max_freq*0.05), xend=x_max, yend=Y_MIN+(max_freq*0.05)  ),linetype="dashed") +
                        geom_rect(mapping=aes(xmin=qn[1], xmax=qn[3], ymin=Y_MIN, ymax=Y_MAX), fill="cyan", color="black") +
                        geom_segment(mapping=aes(  x=qn[2], y=Y_MIN, xend=qn[2], yend=Y_MAX  ), size=1) +
                        geom_segment(mapping=aes(  x=x_min, y=Y_MIN, xend=x_min, yend=Y_MAX  )) +
                        geom_segment(mapping=aes(  x=x_max, y=Y_MIN, xend=x_max, yend=Y_MAX  )) +
                        scale_x_continuous(position="top")
                return(G)
        }
        if(is.factor(Colmn)){# can plot even if NAs are present
                Freq2[,1] <- as.character(Freq2[,1])
                rownames(Freq2) <- NULL
                G <- ggplot(data = Freq2, mapping = aes(x=Colmn, y=Freq)) +
                        geom_bar(stat="identity", fill="black") +
                        xlab(Colmn_name) + ylab("frequency") +
                        scale_x_discrete(position="top")
                return(G)
        }
}


ui <- fluidPage(
        titlePanel(
                h3(textOutput("description"))
        ),
        sidebarLayout(position = "right",
                      sidebarPanel(
                              verbatimTextOutput("hoverinfos"),
                              selectInput("rawdata", paste0("Choose any R Dataset {",length(A),"}"), size = 32, selectize = FALSE, choices = A)
                      ),
                      mainPanel=mainPanel(
                              tabsetPanel(type="tabs",
                                          tabPanel("Basic Info",
                                                   h5("class(Dataset)"),
                                                   verbatimTextOutput("class"),
                                                   h4("Data Summaries"),
                                                   h5("summary(Dataset)"),
                                                   verbatimTextOutput("summary"),
                                                   h5("str(Dataset)"),
                                                   verbatimTextOutput("str")
                                          ),
                                          tabPanel("Full Data",
                                                   verbatimTextOutput("headdata"),
                                                   verbatimTextOutput("taildata"),
                                                   verbatimTextOutput("fulldata")
                                          ),
                                          tabPanel("NA stats",
                                                   h3("NA data statistics"),
                                                   h4("% of NAs per column (column headers are in %s) (column indices in [ ])"),
                                                   h4("Remove/Impute any NAs before proceeding further."),
                                                   verbatimTextOutput("NAs")
                                          ),
                                          tabPanel("Columns",
                                                   h5("https://ggplot2.tidyverse.org/reference/geom_density.html"),
                                                   h5("colnames(Dataset)"),
                                                   verbatimTextOutput("colmnames"),
                                                   h5("names(Dataset)"),
                                                   verbatimTextOutput("colmnames2"),
                                                   selectInput("column", "Choose a column", choices = character(0)),
                                                   h4("columns class and structure"),
                                                   h5("class(Colmn)"),
                                                   verbatimTextOutput("colmclass"),
                                                   h5("str(Colmn)"),
                                                   verbatimTextOutput("colmstr"),
                                                   plotOutput("freqplots")
                                          ),
                                          
                                          
                                          
                                          
                                          
                                          
                                          
                                          
                                          
                                          
                                          
                                          
                                          tabPanel("X-Y plots",
                                                   selectInput("colone", "Choose X-axis column", choices = character(0)),
                                                   selectInput("coltwo", "Choose Y-axis column", choices = character(0)),
                                                   plotOutput("scatplot")
                                                   
                                                   #h4("data class / if condition"),
                                                   #verbatimTextOutput("type")
                                          ),
                                          tabPanel("All",
                                                   verbatimTextOutput("allclasstypes"),
                                                   verbatimTextOutput("allclass")
                                          ),
                                          tabPanel("Tab 2", textOutput("text_output_1"))
                              )
                      )
        )
)







server <- function(input, output, session) {
        
        
        output$allclass <- renderPrint({
                U <- character(0)
                B <- character(0)
                for(i in 1:length(X)){
                        U <- c(U, paste(class(get0(X[i])), collapse="  "))
                        B <- c(B, paste(dim(get0(X[i])), collapse=":"))
                }
                V <- data.frame( class=U, dataset=X, diemens=B )
                V <- V[order(V[,1],V[,2]),]
                options("max.print"=5000)
                V
        })
        
        output$allclasstypes <- renderPrint({
                U <- character(0)
                for(i in 1:length(X)){
                        U <- c(U, paste(class(get0(X[i])), collapse="  "))
                }
                as.data.frame( table(U) )
        })
        
        #ny <- ny[order(ny[,"NA Values"], -ny[,"p-Vals <0.05"], -ny[,"VIFs <5"], ny[,"p-Vals"]),]
        
        observeEvent(input$rawdata,{
                
                output$description <- renderText({
                        Z[match(input$rawdata,X)]
                })
                Dataset <- get0(input$rawdata)
                
                output$class <- renderPrint({  class(Dataset)  })
                output$summary <- renderPrint({  summary(Dataset)  })
                output$str <- renderPrint({  str(Dataset)  })
                output$headdata <- renderPrint({  head(Dataset,10)  })
                output$taildata <- renderPrint({  tail(Dataset,10)  })
                output$fulldata <- renderPrint({  Dataset  })
                    output$NAs <- renderPrint({  NULL  })
                    updateSelectInput(session, "column",  choices = character(0))
                output$colmnames <- renderPrint({  colnames(Dataset)  })
                output$colmnames2 <- renderPrint({  names(Dataset)  })
                    output$colmclass <- renderPrint({  NULL  })
                    output$colmstr <- renderPrint({  NULL  })
                    output$freqplots <- renderPlot({  NULL  })
                    updateSelectInput(session, "colone",  choices = character(0))
                    updateSelectInput(session, "coltwo",  choices = character(0))
                    output$scatplot <- renderPlot({  NULL  })
                
                
                if(is.data.frame(Dataset)){
                        colnems <- colnames(Dataset)
                        output$NAs <- renderPrint({  NA_Data_3( Dataset )  })
                        
                        updateSelectInput(session, "column",  choices = colnems)
                        
                        observeEvent(input$column,{
                                Colmn_name <- input$column
                                Colmn <- Dataset[[Colmn_name]]
                                
                                output$colmclass <- renderPrint({  class(Colmn)  })
                                output$colmstr <- renderPrint({  str(Colmn)  })
                                
                                output$freqplots <- renderPlot({
                                        Distribution(Colmn,Colmn_name)
                                })
                        })
                        
                        updateSelectInput(session, "colone",  choices = colnems)
                        updateSelectInput(session, "coltwo",  choices = colnems)
                        output$scatplot <- renderPlot({
                                
                                xlab <- input$colone
                                ylab <- input$coltwo
                                x1 <- Dataset[[xlab]]
                                x2 <- Dataset[[ylab]]
                                
                                if( is.numeric(x1) & is.numeric(x2) ){
                                        df <- data.frame(x1,x2)
                                        
                                        ## Use densCols() output to get density at each point
                                        x <- densCols(x1,x2, nbin=1000, colramp=colorRampPalette(c("black", "white")))
                                        df$dens <- col2rgb(x)[1,] + 1L
                                        
                                        ## Map densities to colors
                                        cols <-  colorRampPalette(c("#000000", "#00FEFF", "#45FE4F", "#FCFF00", "#FF9400", "#FF3100"))(256)
                                        df$col <- cols[df$dens]
                                        
                                        ## Plot it, reordering rows so that densest points are plotted on top
                                        return(  plot(x2~x1, data=df[order(df$dens),], pch=20, col=col, cex=1, xlab=xlab, ylab=ylab)  )
                                        
                                        #library(ggplot2)
                                        #return(  ggplot(data=df, mapping=aes(x1,x2, colour=col))+geom_point()+scale_colour_identity()  )
                                        
                                }else{ return(NULL) }
                                
                                
                                
                        })
                }
        })
}

shinyApp(ui=ui, server=server)
```
