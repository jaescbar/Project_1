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

I prefer not to answer                         38284.836141

Professional degree                            39503.658863

Secondary school                               40395.148419

I never completed any formal education         44430.660621

Some college/university study without degree   55912.810459

Bachelor’s degree                              56914.358553

Master’s degree                                58250.838766

Primary/elementary school                      62677.337356

Doctoral degree                                78527.933053

This lead us to think that never completing any type of formal education lowers the income and job satisfaction of a person.

## Question 2
To answer the second question a linear regression model was used. By doing this, we can see how changes in the amount of hours worked per week and in the salary change job satisfaction. The results were expected. The amount of hours worked per weak decreases job satisfaction, especificly the increase in one extra hour per week decreases the job satisfaction scale in 0.0571125901. On the other hand, increasing the salary increases job satisfaction. A one dolar increase generates and increase in the job satisfaction scale by 0.00000591338185.

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

Intercept     6.7603415

HoursPerWeek -5.71125901e-02

Salary        5.91338185e-06


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
Intercept              35003.91577279

HoursPerWeek          -554.05365709

Country_United States  58067.90040038

Country_United Kingdom 13817.56257984

Country_Colombia      -20934.88944526

Gender_Male            4959.4156875











## Welcome to GitHub Pages

You can use the [editor on GitHub](https://github.com/jaescbar/Project_1/edit/gh-pages/index.md) to maintain and preview the content for your website in Markdown files.

Whenever you commit to this repository, GitHub Pages will run [Jekyll](https://jekyllrb.com/) to rebuild the pages in your site, from the content in your Markdown files.

### Markdown

Markdown is a lightweight and easy-to-use syntax for styling your writing. It includes conventions for

```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/jaescbar/Project_1/settings). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://docs.github.com/categories/github-pages-basics/) or [contact support](https://support.github.com/contact) and we’ll help you sort it out.
