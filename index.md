# Questions Hidden in the Stack Overflow Surveys

Watching the information inside the dataset Stack Overflow Data - 2017 Survey there are many question one could ask. The dataset has over 50.000 observations and 154 variables. Having said that, there are three questions that interested me the most. The fist one had to do with not having a formal education and its relation to a person’s salary or job satisfaction. The first question ended up being: Not having a formal education affects a person’s job satisfaction and salary? The second question was about what makes a person fill happy at his job, How does hours worked and salary affect a person’s job satisfaction? Finally the third question was abou differences in salary. People that live in the US, UK and Colombia earn a diferent salaries even when we control by gender and by hours worked? Those are the three question that this proyect wants to answer.

## Question 1
To answer the first question a grouping strategy is used. Grouping by the type of education we can see the average income and the average job satisfaction for each group. In this case, we are interested in people that never completed any form of formal education. In job satisfaction this group ranked last, having an average job satisfaction of 6.833333

```markdown
df.groupby(['FormalEducation']).mean()['JobSatisfaction'].sort_values()

df.groupby(['FormalEducation']).mean()['Salary'].sort_values()
```

![](https://github.com/jaescbar/Project_1/blob/main/Images/Imagen%201.PNG)

In salary this group ranked in the lower half, with an average income of 44430.660621.


![](https://github.com/jaescbar/Project_1/blob/main/Images/imagen%202.PNG)

This lead us to think that never completing any type of formal education lowers the income and job satisfaction of a person.

## Question 2
To answer the second question a linear regression model was used. By doing this, we can see how changes in the amount of hours worked per week and in the salary change job satisfaction. The results were expected, not only by logic but seeing a correlation matrix before modelling.

![](https://github.com/jaescbar/Project_1/blob/main/Images/imagen%203.PNG)

The amount of hours worked per weak decreases job satisfaction, especificly the increase in one extra hour per week decreases the job satisfaction scale in 0.0571125901. On the other hand, increasing the salary increases job satisfaction. A one dolar increase generates and increase in the job satisfaction scale by 0.00000591338185.

```markdown
# Variables
variables = df[['JobSatisfaction', 'HoursPerWeek', 'Salary']]
variables = variables.dropna(subset=['Salary', 'JobSatisfaction' ], axis=0)

# Fix numerical values
numericas = variables.select_dtypes(include=['float', 'int']).columns
for col in numericas:
    variables[col].fillna((variables[col].mean()), inplace=True)
        
# Create X and y
y = variables[['JobSatisfaction']]   
X = variables[['HoursPerWeek', 'Salary']]

X_train, X_test, y_train, y_test = train_test_split(X, y, test_size = .30, random_state=123)

modelo = LinearRegression(normalize=True) 

modelo.fit(X_train, y_train) 

y_test_preds = modelo.predict(X_test) 
```

![](https://github.com/jaescbar/Project_1/blob/main/Images/Imagen%205.PNG)


## Question 3
Finally, to answer the third question a linear regression model was used. By doing this, we can see how changing someones country changes his salary. Being in the US increses salary in 58067.90040038, being in the UK increses salary in 13817.56257984 and being in Colombia decreses salary in 20934.88944526 compared to the intercept. This analysis is made controling by gender and by hours worked and has no purchising power parity.


```markdown
# Variables
variables = df[['Salary', 'HoursPerWeek','Country', 'Gender']]
variables = variables.dropna(subset=['Salary','Country', 'Gender'], axis=0)
y = variables[['Salary']]   
variables = variables[['HoursPerWeek','Country', 'Gender']]

# Fix numerical Values
numericas = variables.select_dtypes(include=['float', 'int']).columns
for col in numericas:
    variables[col].fillna((variables[col].mean()), inplace=True)

# fix Categorical Values    
categoricas = variables.select_dtypes(include=['object']).copy().columns
for var in  categoricas:
       variables = pd.concat([variables.drop(var, axis=1), pd.get_dummies(variables[var], prefix=var, prefix_sep='_', drop_first=True)], axis=1)
        
# Create X and y
X = variables[['HoursPerWeek','Country_United States','Country_United Kingdom', 'Country_Colombia', 'Gender_Male']]

X_train, X_test, y_train, y_test = train_test_split(X, y, test_size = .30, random_state=123)

modelo2 = LinearRegression(normalize=True) 

modelo2.fit(X_train, y_train) 

y_test_preds = modelo2.predict(X_test) 

```
![](https://github.com/jaescbar/Project_1/blob/main/Imagen%206.PNG)
