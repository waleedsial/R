
# Air Quality Dataset 
# User Adjusted Histogram
# Real time renderring of Histogram depending upon user selected bin size

# 1 user interface
# 2 Server Component 
# 3 Generate the output & send back to UI 

# Modified from https://shiny.rstudio.com/tutorial/written-tutorial/lesson1/


library(shiny)
data(airquality)

# Define UI for app that draws a histogram ----
ui <- fluidPage(
  
  # App title ----
  titlePanel("Ozone !"),
  
  # Sidebar layout with input and output definitions ----
  # Specify the side bar panel with slider input for number of bins 
  # by defualt step size is 1 in slider input 
  sidebarLayout(
    
    # Sidebar panel for inputs ----
    sidebarPanel(
      
      # Input: Slider for the number of bins ----
      sliderInput(inputId = "bins",
                  label = "Number of bins:",
                  min = 1,
                  max = 50,
                  value = 40, 
                  step = 1)
      
    ),
    
    # Main panel for displaying outputs ----
    mainPanel(
      
      # Output: Histogram ----
      plotOutput(outputId = "distPlot")
      
    )
  )
)

# Define server logic required to draw a histogram ----
server <- function(input, output) {
  
  
  output$distPlot <- renderPlot({
    # Select data
    # Select the bins 
    # Pass them to hist function 
    
    x    <- airquality$Ozone # $ sign means to select the column. 
    x    <- na.omit(x) # omitting the missing data in ozone column 
    bins <- seq(min(x), max(x), length.out = input$bins + 1)
    
    hist(x, breaks = bins, col = "#75AADB", border = "black",  # code is for color blue 
         xlab = "Ozone level",
         main = "Histogram of Ozone level")
    
  })
  
}

# Create Shiny app ----
shinyApp(ui = ui, server = server)