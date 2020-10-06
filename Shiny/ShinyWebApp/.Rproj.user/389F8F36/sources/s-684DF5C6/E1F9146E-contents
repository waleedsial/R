# Predict play or not to play. 
# User will be able to input Outlook, temperature, Humidity, Windy. 
# Application will feature slider & drop downs for input values. 
# 

# Import libraries
library(shiny)
library(shinythemes)
library(data.table)
library(RCurl)
library(randomForest)

# Read weka data
weather <- read.csv(text = getURL("https://raw.githubusercontent.com/dataprofessor/data/master/weather-weka.csv") )

# NOTE 
# When running the code for the model, we may need to convert the char to factors 
# otherwise the random forest model will not work. 
# Reference link : https://stackoverflow.com/questions/39320408/error-in-y-ymean-non-numeric-argument-to-binary-operator-randomforest-r
weather$outlook = factor(weather$outlook)
weather$play = factor(weather$play)
str(weather)


# Build Random Forest model
# All variables 
# play is the output variable which we want to predict. 
model <- randomForest(play ~ ., data = weather, ntree = 500, mtry = 4, importance = TRUE)

# Save model to RDS file
# saveRDS(model, "model.rds")

# Read in the RF model
#model <- readRDS("model.rds")


# UI 
# We set the default values
# Ranges for numeric values
# The labels are how the server accesses the inputvalues. 
# For example, server will take input$Outlook to get the Outlook value. 
ui <- fluidPage(theme = shinytheme("cerulean"),
                
                # Page header
                headerPanel('Should I play Gold or Not ?'),
                
                # Input values
                sidebarPanel(
                  HTML("<h3>Input parameters</h3>"),
                  
                  selectInput("outlook", label = "Outlook:", 
                              choices = list("Sunny" = "sunny", "Overcast" = "overcast", "Rainy" = "rainy"), 
                              selected = "Rainy"),
                  sliderInput("temperature", "Temperature:",
                              min = 64, max = 86,
                              value = 70),
                  sliderInput("humidity", "Humidity:",
                              min = 65, max = 96,
                              value = 90),
                  selectInput("windy", label = "Windy:", 
                              choices = list("Yes" = "TRUE", "No" = "FALSE"), 
                              selected = "TRUE"),
                  
                  actionButton("submitbutton", "Submit", class = "btn btn-primary")
                ),
                
                mainPanel( # this will show the output from server. 
                  tags$label(h3('Status/Output')), # Status/Output Text Box
                  verbatimTextOutput('contents'),
                  tableOutput('tabledata') # Prediction results table
                  
                )
)


# Server


server <- function(input, output, session) {
  
  # Input Data
  datasetInput <- reactive({  
    
    # outlook,temperature,humidity,windy,play
    # We are using the labels to access the input values.
    # Therefore, we want to get the values from the UI input. 
    df <- data.frame(
      Name = c("outlook",
               "temperature",
               "humidity",
               "windy"),
      Value = as.character(c(input$outlook,
                             input$temperature,
                             input$humidity,
                             input$windy)),
      stringsAsFactors = FALSE)
    
    play <- "play"
    df <- rbind(df, play) # combine the play column 
    input <- transpose(df)
    write.table(input,"input.csv", sep=",", quote = FALSE, row.names = FALSE, col.names = FALSE)
    
    test <- read.csv(paste("input", ".csv", sep=""), header = TRUE)
    
    # Factors is necessary for random forest. 
    
    test$outlook <- factor(test$outlook, levels = c("overcast", "rainy", "sunny"))
    
    # Call predict on the entered data. 
    Output <- data.frame(Prediction=predict(model,test), round(predict(model,test,type="prob"), 3))
    print(Output)
    
  })
  
  # Status/Output Text Box
  output$contents <- renderPrint({
    if (input$submitbutton>0) { 
      isolate("Calculation complete.") 
    } else {
      return("Server is ready for calculation.")
    }
  })
  
  # Prediction results table
  output$tabledata <- renderTable({
    if (input$submitbutton>0) { 
      isolate(datasetInput()) 
    } 
  })
  
}


# Create the shiny app

shinyApp(ui = ui, server = server)