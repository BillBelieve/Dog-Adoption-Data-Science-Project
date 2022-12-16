# Dog Adoption Data Science Project
The animal rescue industry will be aided in efficiency through this epic quest for a high R^2

## problem statement:
The operation of running an independent animal shelter is dependent on optimizing the relocation of stray, runaway, or surrendered pets. There are a large number of animals in the system, and the longer they stay in rescue, the more undesirable behavioral traits they establish. If the animal care system is run inefficiently, a larger share of the animals have to be euthanized. While they could have found a new home and had a good life. 

## introduction:
In the United States, every year 3,1 million Dogs enter the animal shelter system. Animal Shelters aren’t government funded, and therefore rely on donations. They are overcrowded and face a multitude of issues regarding stress, limited resources, and disease control. Dogs that spend a significant amount of time in the rescue system tend to develop unattractive behavioral traits to potential adopters, which worsen their chance of finding a forever home. When a dog spends too long in the animal control system the unattractive behavioral may compound to a point where euthanasia is necessary. 

## Goal:
In this research I will explore the possibility of leveraging data science to aid the tattered industry of rehoming dogs. The goal is to make an inference of which variables make a dog better suited to be rehomed. If these dogs then come into a shelter, they may be given priority in the system so that the best suited dogs do not decay under the system to a point where they have to be euthanized. It saves good dogs from being snowed under in a congested system where resources are limited. And the euthanasia rate will be lower with more efficient dog allocation 

## Dataset: https://www.kaggle.com/datasets/velazquezelsa/petadoption
The data has been scraped off a site called petfinder.com. It is an American site on which animal shelters post their dogs which are available for adoption. The data has 26 columns and features descriptive data about the dogs such as the breed, color, and size. As well as data about the condition of the dog; whether they are neutered, declawed, and vaccines are up to date. Lastly there is information about the socialization of the dog. This indicates if the dog is socialized with children, other animals and cats. The dataset has 722 unique entries. of which 30% is still up for adoption, and 70% has already been adopted. There is no data in this set which indicates how many dogs are euthanized. But according to The American Society for the Prevention of Cruelty to Animals, each year 390.000 dogs in animal care are euthanized because of overcrowding and behavior. 

## outcome variables:

time in shelter (difference between publish date of and status changed date)
the amount of time a rescue dog spends in the shelter from the day he was put on Petfinder.com till day he was adopted. The shorter the time spent in shelter, the more success

Predictor variables:

| Descriptive: variable   | option 1      | option 2      |  option 3     | option n      |
| ----------------------- | ------------- | ------------- | ------------- | ------------- |
| Breed                   | Terrier       | Labrador      | Husky         | etc           |
| Mixed                   | Yes           | No            |               |               |
| colour                  | Black         | Brown         | White         | etc           |
| Age                     | Baby          | Young         | Adult         | Senior        |
| gender                  | Male          | Female        |               |               |
| Size                    | Small         | Medium        | Large         |               |

| Attributes              | option 1      | option 2      |
| ----------------------- | ------------- | ------------- |
| neutered                | Yes           | No            |
| house trained           | Yes           | No            |
| declawed                | Yes           | No            |
| special needs           | Yes           | No            |
| shots current           | Yes           | No            |

| Socialized with         | option 1      | option 2      |
| ----------------------- | ------------- | ------------- |
| Children                | Yes           | No            |
| Dogs                    | Yes           | No            |
| Cats                    | Yes           | No            |

## The model:
The statistical model that will be used is a multiple regression. As there will be 1 outcome variable, namely time in shelter. And several factors which determine this outcome. 
After the data set has been cleaned. There will be a selection in the independent variables that have an influence on the outcome variable. 

The process of performing multiple regression consists of: 
To test the significance of the independent variables, i will run an F-test 
confidence interval and t-test to estimate the size of ß per independent variable. 
Calculate the R2adj. to estimate the model fit to the data.
estimation of the standard deviation of the random error.
residual tests

Assumptions of a multiple regression are that the random error is normally distributed, and that all pairs of random errors are independent. This will have to be assessed after the random error is calculated. If the assumptions hold, the model is more valid. 

## potential modeling problem:
assumption violation: patterns and trend in the residual values
multicollinearity - independent variables are correlated among each other. 
extrapolation - predicting y outside the range of independent variables. 
data issues - missing or invalid data

Each of these problems requires its own solution. And I will be alert in respect to these potential problems. 

## Python modules needed: 
Numpy 	- for f test, for making advanced calculations and permutations on the data frame. 
matplotlib 	- for making diagrams, graphs, and any other visualization of the data. 
pandas 		- for making the data frame, and cleaning the dataset. 
scikit-learn 	- for performing statistical tests, and checking the assumptions of the model. 

### borrowed code

for the polynomial model:
https://enjoymachinelearning.com/blog/multivariate-polynomial-regression-python/
https://enjoymachinelearning.com/blog/multivariate-polynomial-regression-python/



### resources:
ASPCA. Characteristics, Challenges of the Shelter Environment. (2020, 28 april). ASPCApro. https://www.aspcapro.org/characteristics-challenges-shelter-environment

Huang, Y. (2022, 7 januari). Animal Adoption-How Data Science Can be Used to Help Animals in Shelter? Medium. https://e-82849.medium.com/animal-adoption-how-data-science-can-be-used-to-help-animals-in-shelter-30b980db7403

S. B. Hansen (2022, 23 juli). PawsLikeMe provides perfect pet matches. Dog’s Best Life. https://dogsbestlife.com/adoption/pawslikeme-provides-perfect-pet-matches/

Wells, D.L. & Graham, L. & Hepper, P.G.. (2002). The Influence of Length of Time in a Rescue Shelter on the Behaviour of Kennelled Dogs. Animal Welfare. 11. 317-325. 

Yadhunath, R. (2021, 2 maart). Saving Animal Lives with Data. Medium. https://towardsdatascience.com/saving-animal-lives-with-data-d815c6e854eb 

Zadeh, A., Combs, K., Burkey, B., Dop, J., Duffy, K. & Nosoudi, N. (2022). Pet analytics: Predicting adoption speed of pets from their online profiles. Expert Systems with Applications, 204, 117596. https://doi.org/10.1016/j.eswa.2022.117596
