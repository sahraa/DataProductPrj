Data Products Project 
========================================================
author: Sahraa
date: July 2016 
autosize: true
**Data Polution (PM25) Trends in selected year range in selected city (between LosAngeles and Baltimore)

Interactive Analysis Report Question
========================================================
In this Application you can see the interactive line chart of PM25 Average Pollution in the two cities of 
- LosAngeles 
-  Baltimore

You can select the city through check box and define the range of the plot x-axis by slider between the range of
- 1999 till 2008 , step= 3 years
** we use previous measured data for this analysis

Slide With Code - Shiny Application
========================================================
inputs are gathered with slider and checkbox

```r
library(webshot)
library(shiny)
p <- shinyApp(
      ui= fluidPage(
        # Application title
        titlePanel("Data Polution (PM25) Trends in selected year range in selected city between LosAngeles and Baltimore"),
        # Sidebar with a slider input for select the maximum year (xaxis max) 
        sidebarLayout(
            sidebarPanel(
                sliderInput("Year",
                            "Select End Year",
                            min = 1999,
                            max = 2008,
                            value = 2008,
                            step=3),
                
                checkboxGroupInput("id2","Please Select a City to Show related Data",
                                   c("Baltimore City"= "Baltimore",
                                     "LosAngeles City" = "LosAngeles"))
            ),

            # Show a plot of the generated distribution
            mainPanel(
                h3('The City You Has Been Selected'),
                verbatimTextOutput("oid2"),
                plotOutput("distPlot")
            )
        )
    ),
    server= function(input, output) {
         output$distPlot <- renderPlot({
            output$oid1 <- renderPrint({input$id1})
            output$oid2 <- renderPrint({input$id2})
            ##Make small data frame
            D1 <- data.frame(City = c(rep("LosAngeles",each=4),rep("Baltimore", each = 4)), 
                             year = c(1999,2002,2005,2008,1999,2002,2005,2008) ,
                             Emissions = c(3931,4373,4601,4101,346,184,130,108))
            ifelse(input$id2=="Baltimore", (D2 <- subset(D1, City=="Baltimore", c("Emissions", "year","City")) ),
                   ( D2 <- subset(D1, City=="LosAngeles", c("Emissions", "year","City"))  )
            )
            
            # draw the line plot for specified city and specified year range
            
            plot(D2$year,D2$Emissions, type="l",col="purple",lwd="6",xlab="Year",ylab="Total PM25 Emissions",xlim=c(1999,input$Year))

        })
    } ,
 options = list(height = 500)

)   
```



Slide With Plot - Sample Plot
========================================================
![plot of chunk unnamed-chunk-2](DDP.rpres-figure/unnamed-chunk-2-1.png)

Summary
========================================================
this Shiny Application consists of two parts
- Shiny Server
- Shiny ui

which gather two input from users and displays interctive graph of air PM25 pollution summary

Shiny Application Link: https://sahra.shinyapps.io/FirstSahra/
