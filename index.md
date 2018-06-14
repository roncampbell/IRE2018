![](https://github.com/roncampbell/IRE2018/blob/master/GatoR.png?raw=true)

# Intro to R

By Ronald Campbell / NBC Owned Television Stations
<ron.campbell@nbcuni.com>

Investigative Reporters & Editors National Conference /
June 14-17, 2018 /
Orlando

First of a two-part class. The second part is taught by T. Christian Miller of Pro Publica. His handout is available at: <tinyurl.com/IRE2018R>

<code>R</code> is a powerful open-source programming and statistical language. Scientists, statisticians and, increasingly, journalists are using it to find answers in mountains of data. People turn to R for everything from analyzing multi-year medical studies to preparing charts and drawing maps. The language itself, known as "base R", can do quite a bit. But computer scientists have vastly expanded its capabilities with more than 12,000 add-on tools or <code>packages</code>. 

The most popular of these packages is a suite of about 20 tools known as the <code>tidyverse</code>. It was developed largely by Hadley Wickham, chief scientist at <code>R Studio</code>, the graphical user interface for R. The tidyverse includes tools for importing, manipulating, editing and visualizing data; we'll be using it extensively in this class.

Before we go further, let's cover a few rules of the road. As a programming language, R is very powerful. You can do a lot of work quickly. The tradeoff is a steep learning curve. While there usually are two or three (or more) ways to do the same task in R, there are an infinite number of ways to do it wrong. Spelling and capitalization are extremely important; the strictest copy editor you ever met was easy going compared with R.

Since R is open-source, it is free. You can download R and most packages at [CRAN](https://cran.r-project.org/), the Comprehensive R Archive Network. Installing packages is a two-step process. If you were to install the tidyverse, for example, you would enter the following commands at the console. Remarks to the right, separated by hash signs, are comments, which the computer ignores.

    > install.packages("tidyverse")     # command to install package; package within quote marks

The crack IRE-NICAR staff has already installed R, R Studio and the tidyverse on the computers we'll be using in this class. But whenever you start R, you must load the packages you need in memory. Here's how you will do that in this class with the tidyverse:

    > library(tidyverse)      # notice no quote marks

We're going to be creating several tables, known in R as <code>data frames</code>, and naming them as we go. We use an arrow-like thing called an <code>assignment operator</code> to do that. Here it is: <code><-</code>. You'll be using it a lot, and there's a shortcut: In Windows, type Alt plus the minus sign. In Mac, type Option plus the minus sign.

We'll also be writing multi-line formulas, joining each line with an odd device called a <code>pipe</code>. It looks like this: <code>%>%</code>. And yes, there's a shortcut: In Windows, type Shift-Control-M; in Mac, type Shift-Command-M.

In R we usually work in "projects" and keep the data in one place. While we won't be creating a project for this class, we will go partway -- we'll set a working directory. The directory we'll use is wherever the IRE-NICAR staff placed our data. (We'll find out before class.) To set the working directory go to the Session menu and click on Set Working Directory. We'll then look for the correct directory. Now it's time to import some data.

    > AlligatorBites <- read_csv("Alligator_Bites.csv")
    > View(AlligatorBites)
    
Pay attention to spelling and capitalization! If you typed small-v "view", you got an error. While we're at it, let's interview the data, starting by getting the number of rows and columns. We can get that with the command dim(), which is short for "dimensions".

    > dim(AlligatorBites)
    [1] 401  25
    
Next let's find out how many of the bites were fatal. We can get the answer with a frequency table on the column Nonfatal_Fatal. Since we're asking for information about a column, not the data frame as a whole, we have to tell R to look for a particular column in a particular data frame. We do that by using a $-sign to connect the column with the data frame.

    > table(AlligatorBites$Nonfatal_Fatal)

    D   F   N 
    5  24 372 
    
There's our answer: 24 fatal bites, 372 nonfatal and five, well, I don't know what those five are. Maybe the person bit the gator!

How many of the victims were males, and how many were female? Again, a frequency table will answer the question. See if you can do this on your own.

    > table(AlligatorBites$Sex)

     F   M 
    77 324 
    
Don't forget to attach the data frame to the column name.

How old is the typical bite victim? Notice that the Age column has several NA's. NA stands for "Not Available", meaning the victim's age is unknown. NA's are not - repeat, underscore, not - the same as zero; we have no idea what they are. So we have to tell R to ignore them. We do that with the command na.rm = TRUE, which means "remove all NA's". This overrides R's default behavior.

    > median(AlligatorBites$Age, na.rm=TRUE)
    [1] 34

Is there a difference in the median age of male and female victims? We can find out by adding the variable Sex and using the function group_by. We'll also use the pipe tool that we introduced earlier.

    AlligatorBites %>% 
        group_by(Sex) %>% 
        summarise(MedAge = median(Age, na.rm=TRUE))
        # A tibble: 2 x 2
    Sex   MedAge
    <chr>  <dbl>
    1 F        37.
    2 M        34.
    
A tibble is a type of data frame. You may have noticed how I spelled "summarise": I'm using dplyr, one of the best tools in the tidyverse. The author, Hadley Wickham, is from New Zealand and favors (favours?) British spellings. He kindly allows American spellings such as "summarize" and "color", but I've noticed that "summarize" with a "z" sometimes triggers errors, so I always use "summarise" with an "s".    
    
Notice that column, Gator_Fed? Some people like to feed the alligators. This is not a good idea.

Let's look for people who fed gators and got bitten for their trouble. We'll use the filter function to find them. Then we'll group by sex. Finally, we'll count them and calculate the median age by sex. Any bets on which sex is more likely to feed gators?

A couple of points before we do this exercise: First, while we occasionally assign a variable with an equal sign (and more often with the assignment operator), we specify values for variables with a double equal sign (==). (If you're having trouble wrapping your head around that concept, here's an example: variable = "weather", value == "cloudy".)  Second, in R and in most statistical languages the standard shortcut for "the number of persons or things" is "n" or "n()". 

    AlligatorBites %>% 
        filter(Gator_Fed == "Y") %>% 
        group_by(Sex) %>% 
        summarise(Count = n(),
                MedAge = median(Age, na.rm=TRUE))
    # A tibble: 2 x 3
    Sex   Count MedAge
    <chr> <int>  <dbl>
    1 F        12    45.
    2 M        50    38. 
    
In this script, the variable Gator_Fed had two possible values, "Y" or "N"; we specified "Y", using a double-equal sign. In the summarise portion, we created a new variable, Count, to hold the number of persons in each sex who were bitten after feeding a gator. And as we expected, four times more men than women thought it would be a good idea to feed a gator - and ended up literally feeding a gator!

How big are the gators that enjoy human snacks? Again, we have to account for missing values by removing the NA's from the mix. This time we'll calculate the mean, or mathematical average.

    > mean(AlligatorBites$Length_Feet, na.rm=TRUE)
    [1] 7.121622
    
We have 70 years worth of alligator bite records. In which years did the most bites occur? We have a column called “Year”, and we can group on that to find the answer. But we really don't want to pick through seven decades worth of records. R can sort them for us. Better yet, it can display the top years. 

    > AlligatorBites %>% 
         group_by(Year) %>% 
         summarise(Count = n()) %>% 
         arrange(desc(Count)) %>%        # sorting command
         top_n(10)                       # display top 10
    Selecting by Count
    # A tibble: 13 x 2
    Year Count
    <int> <int>
    2001    16
    2013    15
    1977    14
    2007    14
    1986    13
    1990    13
    1995    13
    2000    13
    2002    13
    1993    12
    2004    12
    2006    12
    2017    12

What time of day do alligators bite most often? The Time column gives us time on a 24-hour scale, so it’s ideal for our purposes. We can answer our question using ggplot, a graphic program built into the tidyverse.

    > ggplot(AlligatorBites, aes(Time)) +
         geom_histogram()
    
ggplot can build bar charts, pie charts, line charts and maps as well as histograms, and it can do all these things in a few lines of code. Here it creates a histogram by taking the data frame (AlligatorBites), using the column Time as the aesthetic ("aes" in ggplot-speak) and adding a "geom" or geometric object. A histogram is useful for frequencies.

![](https://github.com/roncampbell/IRE2018/blob/master/BitesByTime.png?raw=true)

The column Water_Body tells us where the gator calls home. We can see at a glance that there are many choices. But we can find the biggest gator holes with a script. In R Studio, click on the icon with a green plus-sign at top left; in the drop-down menu, the first item will be "R Script". Click on that. 

Before writing the script, let's point out something. The "History" tab keeps a record of your current session - each command, in the order you enter it. If you want to re-enter a command, just click on the appropriate line in the history panel, and it will appear in the console. This is an incredibly handy feature. You can save the history indefinitely along with the rest of your workspace with the command save.image("xxx.RData") where "xxx" is the name you give work. When you resume work, type load("xxx.RData") in the console.

If R remembers each command, why write a script? Here are two answers. The console becomes a pretty awkward scripting tool after five or six lines; remember - one typo, and the command fails. I've written 50- and 80-line scripts. Second, you might want to recycle your scripts, modifying them slightly for new data. You simply can't do that in the console. In the script editor, it's a breeze.

Here's our script:

    library(tidyverse)                  # load just in case
    GatorLakes <- AlligatorBites %>%    # create new data frame
        group_by(Water_Body) %>% 
        summarise(count = n()) %>% 
        arrange(desc(count)) %>% 
        top_n(25)

And here are the top results:

    > GatorLakes
    # A tibble: 40 x 2
    Water_Body                   count
    <chr>                        <int>
    Unnamed pond                   100
    Unnamed canal                   20
    Unnamed retention pond          16
    NA                              11
    Juniper Creek                    4
    Lake Okeechobee - Okeechobee     4
    Unnamed lake                     4
    Alexander Springs                3
    Lake Osborne                     3
    Lake Tarpon                      3
