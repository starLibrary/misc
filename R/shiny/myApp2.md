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

ui <- dashboardPage(skin = "green",
        dashboardHeader(title = "Basic dashboard"),
        dashboardSidebar(
                         sidebarMenu(
                                     menuItem("Dashboard", tabName = "dashboard", icon = icon("dashboard")),
                                     menuItem("Widgets", tabName = "widgets", icon = icon("th")),
                                     menuItem("Dashboard template", tabName = "dashtem"),
                                     menuItem("Shiny Tutorial",icon = icon("archway"),
                                              menuSubItem("render", tabName = "render"),
                                              menuSubItem("reactive", tabName = "reactive"),
                                              menuSubItem("isolate", tabName = "isolate"),
                                              menuSubItem("observeEvent", tabName = "observeEvent"),
                                              menuSubItem("observe", tabName = "observe"),
                                              menuSubItem("eventReactive", tabName = "eventReactive"),
                                              menuSubItem("reactiveValues", tabName = "reactiveValues"),
                                              menuSubItem("Summary", tabName = "Summary")
                                     )
                                     
                         )
        ),
        dashboardBody(
                      tabItems(
                               tabItem(tabName = "dashboard"),
                               
                               tabItem(tabName = "widgets",
                                       h2("Widgets tab content"),
                                       verbatimTextOutput("wig"),
                                       box(
                                               title = "Inputs", background = "black",
                                               collapsible = TRUE, status = "success",
                                               "Box content here", br(), "More box content",
                                               sliderInput("slider", "Slider input:", 1, 100, 50),
                                               textInput("text", "Text input:"),
                                               br(),
                                               selectInput("select_2", "Select a choice (type 2)", size = 5, selectize = FALSE,
                                                           choices = c("A","B","C","D","E","F","G","H"))
                                       )
                               ),
                               
                               tabItem(tabName = "dashtem",
                                       'library(shiny)',br(),
                                       'library(shinydashboard)',br(),br(),
                                       'ui <- dashboardPage(skin = "green",',br(),HTML("&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"),
                                       'dashboardHeader(title = "Basic dashboard"),',br(),HTML("&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"),
                                       'dashboardSidebar(',br(),HTML("&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"),
                                       'sidebarMenu(',br(),HTML("&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"),
                                       'menuItem("Dashboard", tabName = "dashboard", icon = icon("dashboard")),',br(),HTML("&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"),
                                       'menuItem("Widgets", tabName = "widgets", icon = icon("th")),',br(),HTML("&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"),
                                       'menuItem("Shiny Tutorial", icon = icon("archway"),',br(),HTML("&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"),
                                       'menuSubItem("dashboard template", tabName = "dashtem"),',br(),HTML("&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"),
                                       'menuSubItem("example-1", tabName = "example-1"),',br(),HTML("&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"),
                                       'menuSubItem("example-2", tabName = "example-2")',br(),HTML("&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"),
                                       ')',br(),HTML("&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"),
                                       ')',br(),HTML("&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"),
                                       '),',br(),HTML("&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"),
                                       'dashboardBody(',br(),HTML("&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"),
                                       'tabItems(',br(),HTML("&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"),
                                       'tabItem(tabName = "dashboard"),',br(),HTML("&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"),
                                       'tabItem(tabName = "widgets",',br(),HTML("&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"),
                                       'h2("Widgets tab content")',br(),HTML("&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"),
                                       '),',br(),HTML("&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"),
                                       'tabItem(tabName = "dashtem")',br(),HTML("&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"),
                                       ')',br(),HTML("&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"),
                                       ')',br(),
                                       ')',br(),br(),
                                       'server <- function(input, output) { ',HTML("&nbsp;&nbsp;"),'}',br(),br(),
                                       'shinyApp(ui, server)'
                               ),
                               
                               tabItem(tabName = "render",
                                       h3("Put input$* inside render*() to make it responsive/reactive"),
                                       sidebarLayout(position = "right",
                                                     sidebarPanel(
                                                                  sliderInput(inputId = "num2",
                                                                              label = "Choose a number",
                                                                              value = 25, min = 1, max = 100
                                                                  ),
                                                                  'library(shiny)',br(),br(),
                                                                  'ui <- fluidPage(',br(),HTML("&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"),
                                                                  'sliderInput(inputId = "num",',br(),HTML("&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"),
                                                                  'label = "Choose a number",',br(),HTML("&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"),
                                                                  'value = 25, min = 1, max = 100),',br(),HTML("&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"),
                                                                  'plotOutput("hist")',br(),
                                                                  ')',br(),br(),
                                                                  'server <- function(input, output){',br(),HTML("&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"),
                                                                  'output$hist <- renderPlot({',br(),HTML("&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"),
                                                                  'hist(rnorm(input$num))',br(),HTML("&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"),
                                                                  '})',br(),
                                                                  '}',br(),br(),
                                                                  'shinyApp(ui=ui, server=server)'
                                                     ),
                                                     mainPanel(plotOutput("hist2"),br(),
                                                               tags$img(src="render.bmp", height="100%", width="100%")
                                                     )
                                       )
                               ),
                               
                               tabItem(tabName = "reactive",
                                       h3("Modularize code with reactive()"),
                                       sidebarLayout(position = "right",
                                                     sidebarPanel(
                                                                  sliderInput(inputId = "num3",
                                                                              label = "Choose a number",
                                                                              value = 25, min = 1, max = 100
                                                                  ),
                                                                  'library(shiny)',br(),br(),
                                                                  'ui <- fluidPage(',br(),HTML("&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"),
                                                                  'sliderInput(inputId = "num",',br(),HTML("&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"),
                                                                  'label = "Choose a number",',br(),HTML("&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"),
                                                                  'value = 25, min = 1, max = 100),',br(),HTML("&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"),
                                                                  'plotOutput("hist"),',br(),HTML("&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"),
                                                                  'verbatimTextOutput("stats")',tags$br(),
                                                                  ')',br(),br(),
                                                                  'server <- function(input, output){',br(),HTML("&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"),
                                                                  'data <- reactive({',br(),HTML("&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"),
                                                                  'rnorm(input$num)',br(),HTML("&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"),
                                                                  '})',br(),HTML("&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"),
                                                                  'output$hist <- renderPlot({',br(),HTML("&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"),
                                                                  'hist(rnorm(input$num))',br(),HTML("&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"),
                                                                  '})',br(),HTML("&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"),
                                                                  'output$stats <- renderPrint({',br(),HTML("&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"),
                                                                  'summary(data())',br(),HTML("&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"),
                                                                  '})',br(),
                                                                  '}',br(),br(),
                                                                  'shinyApp(ui=ui, server=server)'
                                                     ),
                                                     mainPanel(
                                                               plotOutput("hist3"),
                                                               verbatimTextOutput("stats3"),br(),
                                                               tags$img(src="reactive.bmp", height="100%", width="100%")
                                                     )
                                       )
                               ),
                               
                               tabItem(tabName = "isolate",
                                       h3("Delay reactions by isolate()"),
                                       sidebarLayout(position = "right",
                                                     sidebarPanel(
                                                                  sliderInput(inputId = "num4", 
                                                                              label = "Choose a number", 
                                                                              value = 25, min = 1, max = 100
                                                                  ),
                                                                  textInput(inputId = "title4", 
                                                                            label = "Write a title",
                                                                            value = "Histogram of Random Normal Values"
                                                                  ),
                                                                  'library(shiny)',br(),br(),
                                                                  'ui <- fluidPage(',br(),HTML("&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"),
                                                                  'sliderInput(inputId = "num",',br(),HTML("&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"),
                                                                  'label = "Choose a number",',br(),HTML("&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"),
                                                                  'value = 25, min = 1, max = 100),',br(),HTML("&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"),
                                                                  'textInput(inputId = "title",',br(),HTML("&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"),
                                                                            'label = "Write a title",',br(),HTML("&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"),
                                                                            'value = "Histogram of Random Normal Values"),',br(),HTML("&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"),
                                                                  'plotOutput("hist")',br(),
                                                                  ')',br(),br(),
                                                                  'server <- function(input, output){',br(),HTML("&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"),
                                                                  'output$hist <- renderPlot({',br(),HTML("&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"),
                                                                  'hist(rnorm(input$num),',br(),HTML("&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"),
                                                                  'main = ',strong('isolate('),'input$title',strong(')'),br(),HTML("&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"),
                                                                  ')',br(),HTML("&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"),
                                                                  '})',br(),
                                                                  '}',br(),br(),
                                                                  'shinyApp(ui=ui, server=server)'
                                                     ),
                                                     mainPanel(plotOutput("hist4"),br(),
                                                               tags$img(src="isolate.bmp", height="100%", width="100%")
                                                     )
                                       )
                               ),
                               
                               tabItem(tabName = "observeEvent",
                                       h3("Trigger code execution on server"),
                                       sidebarLayout(position = "right",
                                                     sidebarPanel(
                                                                  actionButton(inputId = "clicks5",
                                                                               label = "Click me"),HTML("&nbsp;&nbsp;"),
                                                                  "Check the server terminal after clicking",
                                                                  br(),br(),br(),
                                                                  'library(shiny)',br(),br(),
                                                                  'ui <- fluidPage(',br(),HTML("&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"),
                                                                  'actionButton(inputId = "clicks",',br(),HTML("&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"),
                                                                  'label = "Click me")',br(),
                                                                  ')',br(),br(),
                                                                  'server <- function(input, output){',br(),HTML("&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"),
                                                                  'observeEvent(input$clicks, {',br(),HTML("&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"),
                                                                  'print(as.numeric(input$clicks))',br(),HTML("&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"),
                                                                  '})',br(),
                                                                  '}',br(),br(),
                                                                  'shinyApp(ui=ui, server=server)'
                                                     ),
                                                     mainPanel(tags$img(src="observeEvent.bmp", height="100%", width="100%"))
                                       )
                               ),
                               
                               tabItem(tabName = "observe",
                                       h3("Same as observeEvent(), but shorter syntax"),
                                       sidebarLayout(position = "right",
                                                     sidebarPanel(),
                                                     mainPanel(tags$img(src="observe.bmp", height="100%", width="100%"))
                                       )
                               ),
                               
                               tabItem(tabName = "eventReactive",
                                       h3("Delay reactions by eventReactive()"),
                                       sidebarLayout(position = "right",
                                                     sidebarPanel(
                                                                  sliderInput(inputId = "num6", 
                                                                              label = "Choose a number", 
                                                                              value = 25, min = 1, max = 100),
                                                                  actionButton(inputId = "go6", label = "Update"),
                                                                  br(),br(),br(),
                                                                  'library(shiny)',br(),br(),
                                                                  'ui <- fluidPage(',br(),HTML("&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"),
                                                                  'sliderInput(inputId = "num",',br(),HTML("&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"),
                                                                  'label = "Choose a number",',br(),HTML("&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"),
                                                                  'value = 25, min = 1, max = 100),',br(),HTML("&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"),
                                                                  'actionButton(inputId = "go",',br(),HTML("&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"),
                                                                  'label = "Update"),',br(),HTML("&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"),
                                                                  'plotOutput("hist")',br(),
                                                                  ')',br(),br(),
                                                                  'server <- function(input, output){',br(),HTML("&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"),
                                                                  'data <- eventReactive(input$go, {',br(),HTML("&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"),
                                                                  'rnorm(input$num)',br(),HTML("&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"),
                                                                  '})',br(),HTML("&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"),
                                                                  'output$hist <- renderPlot({',br(),HTML("&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"),
                                                                  'hist(data())',br(),HTML("&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"),
                                                                  '})',br(),
                                                                  '}',br(),br(),
                                                                  'shinyApp(ui=ui, server=server)'
                                                     ),
                                                     mainPanel(plotOutput("hist6"),br(),
                                                               tags$img(src="eventReactive.bmp", height="100%", width="100%")
                                                     )
                                       )
                               ),
                               
                               tabItem(tabName = "reactiveValues",
                                       h3("....."),
                                       sidebarLayout(position = "right",
                                                     sidebarPanel(
                                                             actionButton(inputId = "norm7", label = "Normal"),
                                                             actionButton(inputId = "unif7", label = "Uniform"),
                                                             br(),br(),br(),
                                                             'library(shiny)',br(),br(),
                                                             'ui <- fluidPage(',br(),HTML("&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"),
                                                             'actionButton(inputId = "norm",',br(),HTML("&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"),
                                                             'label = "Normal"),',br(),HTML("&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"),
                                                             'actionButton(inputId = "unif",',br(),HTML("&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"),
                                                             'label = "Uniform"),',br(),HTML("&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"),
                                                             'plotOutput("hist")',br(),
                                                             ')',br(),br(),
                                                             'server <- function(input, output){',br(),HTML("&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"),
                                                             'rv <- reactiveValues(data = rnorm(100))',br(),br(),HTML("&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"),
                                                             'observeEvent(input$norm, {',br(),HTML("&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"),
                                                             'rv$data <- rnorm(100) })',br(),HTML("&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"),
                                                             '})',br(),HTML("&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"),
                                                             'observeEvent(input$norm, {',br(),HTML("&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"),
                                                             'rv$data <- runif(100) })',br(),HTML("&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"),
                                                             '})',br(),br(),HTML("&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"),
                                                             'output$hist <- renderPlot({',br(),HTML("&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"),
                                                             'hist(data())',br(),HTML("&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"),
                                                             '})',br(),
                                                             '}',br(),br(),
                                                             'shinyApp(ui=ui, server=server)'
                                                     ),
                                                     mainPanel(plotOutput("hist7"),br(),
                                                               tags$img(src="reactiveValues.bmp", height="100%", width="100%")
                                                     )
                                       )
                               ),
        
                               tabItem(tabName = "Summary",
                                       fluidRow(
                                               column(4,tags$img(src="isolate.bmp", height="100%", width="100%")),
                                               column(4,tags$img(src="observe.bmp", height="100%", width="100%")),
                                               column(4,tags$img(src="observeEvent.bmp", height="100%", width="100%"))
                                       ),
                                       fluidRow(
                                               column(4,tags$img(src="reactiveValues.bmp", height="100%", width="100%")),
                                               column(4,tags$img(src="reactive.bmp", height="100%", width="100%")),
                                               column(4,tags$img(src="eventReactive.bmp", height="100%", width="100%"))
                                       )
                               )
                      )
        )
)

server <- function(input, output){
        
        output$hist2 <- renderPlot({
                hist(rnorm(input$num2))# use input inside render to be reactive
        })
        
        
        
        
        
        data3 <- reactive({
                rnorm(input$num3)
        })
        output$hist3 <- renderPlot({
                hist(data3())
        })
        output$stats3 <- renderPrint({
                summary(data3())
        })
        
        
        
        
        
        output$hist4 <- renderPlot({
                hist(rnorm(input$num4), main = isolate(input$title4))
        })
        
        
        
        
        
        observeEvent(input$clicks5, {
                print(as.numeric(input$clicks5))
        })
        
        
        
        
        
        data6 <- eventReactive(input$go6, {
                rnorm(input$num6)
        })
        output$hist6 <- renderPlot({
                hist(data6())
        })
        
        
        
        
        
        rv7 <- reactiveValues(data7 = rnorm(100))
        
        observeEvent(input$norm7, { rv7$data7 <- rnorm(100) })
        observeEvent(input$unif7, { rv7$data7 <- runif(100) })
        
        output$hist7 <- renderPlot({
                hist(rv7$data7)
        })
        
        
        
        
        output$wig <- renderPrint({
                "library(shiny)

                ui <- fluidPage('Hello World')
                
                server <- function(input, output){}
                
                shinyApp(ui=ui, server=server)"
        })
        
        
}

shinyApp(ui, server)
```
