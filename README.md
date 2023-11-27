# logistic_growth
Question 1: R scripts for a reproducible analysis of logistic growth
- Investigation of the dynamics of E.coli population growth using data from experiment1.csv.

Plotting logistic growth data:

- Reading in CSV file

growth_data <- read.csv("experiment1.csv")

- installing + loading ggplot2

install.packages("ggplot2)

library(ggplot2)

- Creating scatter plot with variables N and t

ggplot(aes(t,N), data = growth_data) +

geom_point() +

xlab("t") +

ylab("y") +

theme_bw()

- Performing log function to graph

ggplot(aes(t,N), data = growth_data) +

geom_point() +

xlab("t") +

ylab("y") +

scale_y_continuous(trans='log10')  

- Seen in the plots that the populations exhibit logisitc growth with a period of exponenial growth at the start. As resources become scarce the poulation growth plateues at the carrying capacity.
- By observation of the graph we can see already that N0 = 879.
- The next step is to estimate the growth rate (r) by observing the part of the curve that is exponential.

To install and load the packages:

install.packages("dplyr")

library(dplyr)

Data must be uploaded - used experiment1.csv
growth_data <- read.csv("experiment1.csv)

Case 1 -> K>N0, t is small
- In this case the carrying capacity K is much larger than the population size N. - This is so it is possible to estimate the initial population size and gradient.
- The following line of code is filtering the data and making the model linear.
- The subset only contains values of N above t<1000 this is because it is still in it's exponential phase at t=1000.

data_subset1 <- growth_data %>% filter(t<1000) %>% mutate(N_log = log(N))

model1 <- lm(N_log ~ t, data_subset1)

summary(model1)

- The summary produces an estimate for t in the coefficiencts which is
  = 1.004e-02.
- Which is the slope of model or the growth rate (r).

Case 2 -> N(t) = K
- In this case the carrying capacity is equal to population size.
-  Can estimate carrying capacity (K) by isolating the part of curve where it has plateued.
- In these lines of code I am creating a subset of data where the values of t are greater than 3000. This value has been chosen because beyond this point the population has reached its plateu. As well as fitting a linear model.
  
data_subset2 <- growth_data %>% filter(t>3000)

model2 <- lm(N ~ 1,data_subset2) summary(model2)

- The estimate for intercept represents K = 6.000e+10

We now have the following estimates:
N0 = 879
r = 1.004e-02
K = 6.000e+10

- Defining logistic growth model

logistic_fun <- function(t) {

N <- (N0Kexp(rt))/(K-N0+N0exp(r*t))

return(N)
}

- Definition of parameteres of the logistic growth model
N0 <-  #
r <-  #
K <-  #

- Plotting model and comparing it against the data 

ggplot(aes(t,N), data = growth_data) +

geom_function(fun=logistic_fun, colour="red") +

geom_point() +

scale_y_continuous(trans='log10')

Results:
In this exercise it has been possible to estimate the initial population size, growth rate and the carrying capacity. The data appears to align with the model but there are small differences in the values chosen and the actual population sizes. The starting population size estimate is too large which causes a lag in the model against the data.


Question 2: Use your estimates of  and  to calculate the population size at  = 4980 min, assuming that the population grows exponentially. How does it compare to the population size predicted under logistic growth?

The estimates for the variables are:
N0 = 879
r = 1.004e-02
t = 4980min

By using N(t) = N0e^rt when population is growing exponentially 

N(t) = 879 x e^((1.004e-02) x 4980)
N(t) = 

Then looking at the population size at t = 4980 under a logisitc growth model when K = 99979341


(20 points) Add an R script to your repository that makes a graph comparing the exponential and logistic growth curves (using the same parameter estimates you found). Upload this graph to your repo and include it in the README.md file so it can be viewed in the repo homepage.
