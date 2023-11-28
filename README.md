# logistic_growth
Question 1: R scripts for a reproducible analysis of logistic growth
- Investigation of the dynamics of E.coli population growth using data from experiment1.csv.

Plotting logistic growth data:

- Reading in CSV file

growth_data <- read.csv("experiment2.csv")

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

- The next step is to estimate the growth rate (r) by observing the part of the curve that is exponential.

To install and load the packages:

install.packages("dplyr")

library(dplyr)

Data must be uploaded - used experiment2.csv
growth_data <- read.csv("experiment2.csv")

Case 1 -> K>N0, t is small
- In this case the carrying capacity K is much larger than the population size N. - This is so it is possible to estimate the initial population size and gradient.
- The following line of code is filtering the data and making the model linear.
- The subset only contains values of N above t<541 this is because it is still in it's exponential phase at t=541.

data_subset1 <- growth_data %>% filter(t<541) %>% mutate(N_log = log(N))

model1 <- lm(N_log ~ t, data_subset1)

summary(model1)

- The summary produces an estimate for r in the coefficiencts which is
  = 0.025874
- Which is the slope of model or the growth rate (r).
- C=ln(N0) thus N0 = e^8.186598

Case 2 -> N(t) = K
- In this case the carrying capacity is equal to population size.
-  Can estimate carrying capacity (K) by isolating the part of curve where it has plateued.
- In these lines of code I am creating a subset of data where the values of t are greater than 541. This value has been chosen because beyond this point the population has reached its plateu. As well as fitting a linear model.
  
data_subset2 <- growth_data %>% filter(t>541)

model2 <- lm(N ~ 1,data_subset2) summary(model2)

- The estimate for intercept represents K = 999877492

We now have the following estimates:
N0 = e^8.186598
r = 0.025874
K = 999877492

- Defining logistic growth model

logistic_fun <- function(t) {

N <- (N0Kexp(rt))/(K-N0+N0exp(r*t))

return(N)
}

- Plotting model and comparing it against the data 

ggplot(aes(t,N), data = growth_data) +

geom_function(fun=logistic_fun, colour="red") +

geom_point() +

scale_y_continuous(trans='log10')

Results:
In this exercise it has been possible to estimate the initial population size, growth rate and the carrying capacity. The data appears to align with the model but there are small differences in the values chosen and the actual population sizes. The starting population size estimate is too large which causes a lag in the model against the data.


Question 2: Use your estimates of  and  to calculate the population size at  = 4980 min, assuming that the population grows exponentially. How does it compare to the population size predicted under logistic growth?

The estimates for the variables are:
N0 = e^8.186598
r = 0.025874
t = 4980min

By using N(t) = N0e^rt when population is growing exponentially 

N(t) = 3.28x10^59
This value exceeds our estimated K which is 999877492. The exponential model fails to take into account factors that limit the population growth. Density dependent limiting factors such as nutrients and space cap the population at it's carrying capacity.

Question 3:
Add an R script to your repository that makes a graph comparing the exponential and logistic growth curves (using the same parameter estimates you found). Upload this graph to your repo and include it in the README.md file so it can be viewed in the repo homepage.

![Image 28-11-2023 at 23 27](https://github.com/BioBabe2002/logistic_growth/assets/150148922/5ba75f0b-6a87-4dc6-82b1-69ba725bf316)



