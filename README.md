# Do happier countries drink more or less than sad countries?

![Drinkingtogether](Images/drinkingtogether.png)

   * 111 countries across 9 regions. 
   * Determine what factors make people happy?
   * How does happiness compare across the world?
   * What differences between countries cause certain countries to be happier?
   * As intermediate conclusion could be that drinking affects happiness. However, as we factor in all categories it will show us the big picture on what factors truly contribute to an individual’s happiness.

### Happiness Score by Region

![happinessscore](Figures/Happiness_Score_by_Region.png)

  * What is Happiness score? We looked at 2 data sets from World Happiness Report and Happiness And Alcohol consumption report. 
  * A metric measured by asking sampled people the following question: "How would you rate your happiness on a 1-10 scale”...
  * As we unfold our data findings, we will examine Happiness Score to various contributors SUCH AS: Monetary influences, Alcohol and Socioeconomic factors. 
  * Australians, New Zealanders and North Americans are the Happiest people, while Sub- Saharan Africa along with Central and Eastern Europe are the least happy people.

### GDP Per Capita by Region

![gdppercapita](Figures/GDP_Per_Capita_by_Region.png)

  * Gross domestic product (GDP) is a monetary measure of the market value of all the final goods and services produced in a specific time period
  * North America Highest and Lowest GDP Sub- Saharan African

## Digging Deeper into the Data

  * Let’s dig deeper to find out more about happiness and why some countries are happier than others?
  * We found a dataset that included some information into what made up the Happiness Score for Countries/Regions . We merged the data to correlate with how these can possibly affect alcohol consumption .

```jupyter
top_bottom_df.groupby(["Region"]).mean().plot(kind = "pie", y = "Life Choices", autopct='%1.1f%%',figsize=(9,9), textprops={'color':"w"})
plt.ylabel("")
plt.title("Regional Freedom of Life Choices", fontsize = 20)
plt.legend(bbox_to_anchor=(1.05, 1.0), loc='upper left')
plt.tight_layout()

plt.savefig("Figures/Freedomof_life_choices.png")
plt.show
```

![Freedomoflifechoices](Figures/Freedomof_life_choices.png)

![socialsuport](Figures/Social_Support.png)

![perceptioncorruption](Figures/percofcorruption.png)

## Alcohol Consumption by Regions
```Jupyter
# group bar chart consumption by region
region_all_graph = region_group_all_consumption.plot(kind = "bar", figsize = (15,8))
column_names = region_group_all_consumption["Wine (Per Capita)"], ["Beer (Per Capita)"], ["Spirit (Per Capita)"], ["Happiness Score"]
plt.title("Alcohol Consumption by Regions of Top and Bottom 10 Countries", fontsize = 20)
plt.xlabel("Regions", fontsize = 20)
plt.ylabel("Consumption of Alcohol Per Capita", fontsize = 20)

plt.tight_layout()
plt.savefig("Figures/t_b_10_region_consumption_bar.png")

plt.show
```
![barchartallconcumption](Figures/t_b_10_region_consumption_bar.png)

  * You have seen where alcohol consumption stands individually; Here we have a collective view on how it looks with all the Top/Bottom Ten Happiest countries. Another quick take away we have here is that Wine and Beer are in it for the number 1 spot! 

## Analyzing the Data
  * Used Linear regression on scatter plots to analyze trends
  * .groupby was used to group the data by region for analysis
  * Bar charts and pie plots
  * R-value was used on scatter plots

```jupyter
#scatter plot of gdp and happiness for all countries

#set up scatterplot
plt.figure(figsize = (15,5))
sns.scatterplot(data = happiness_sort_df, x = "GDP (Per Capita)", y = "Happiness Score", palette = "turbo", hue = "Region")
#calc regression
x_values = happiness_sort_df["GDP (Per Capita)"]
y_values = happiness_sort_df["Happiness Score"]
(slope, intercept, rvalue, pvalue, stderr) = linregress(x_values, y_values)
regress_values = x_values * slope + intercept
corr=st.pearson(x_values, y_values)[0]
print(f"the correlation is {corr:.2f}")
line_eq = "y = " + str(round(slope,2)) + "x + " + str(round(intercept,2))
plt.plot(x_values,regress_values,"r-")
plt.annotate(line_eq,(0,2),fontsize=5,color="red")
print(f"The r-squared is: {rvalue**2}")

#clean up scatter plot for looks
plt.title("Happiness Score and GDP (Per Capita) for all Countries", fontsize = 20)
plt.xlabel("Happiness Score", fontsize = 15)
plt.ylabel("GDP Per Capita", fontsize = 15)

plt.savefig("Figures/all_happy_gdp_scatter.png")
plt.show()
```

![all_happy_gdp_scatter](Figures/all_happy_gdp_scatter.png)

## Challenges and Victories

  * Sorting the data in order to get the data for the top and bottom 10 countries
```python
# top 10 data frame
top_df = happiness_sort_df.nlargest(10, "Happiness Score")
top_df
```

```python
# Bottom 10 data frame
bottom_df = happiness_sort_df.nsmallest(10, "Happiness Score")
sort_bottom_df = bottom_df.sort_values("Happiness Score", ascending = False)
sort_bottom_df
```

  * Moving the legend box from overlapping the pie chart
```python
top_bottom_df.groupby(["Region"]).mean().plot(kind = "pie", y = "Perceptions of Corruption",  autopct='%1.1f%%',figsize=(9,9), textprops={'color':"w"})
plt.ylabel("")
plt.title("Region Perceptions of Corruption", fontsize = 20)
plt.legend(bbox_to_anchor=(1.05, 1.0), loc='upper left')
plt.tight_layout()

plt.savefig("Figures/percofcorruption.png")
plt.show
```

  * Understanding how seaborn works and debugging undefined variable
```python

plt.figure(figsize = (15,6))
plt.bar(top_bottom_df["Country"], top_bottom_df["Wine (Per Capita)"])
plt.title("Top and Bottom 10 Happiest Countries & Wine ", fontsize = 20)
sns.barplot(x ='Country', y= 'Wine (Per Capita)', data = top_bottom_df, palette = 'turbo')
sns.set_theme(style="whitegrid")
plt.xticks(rotation = 60)
plt.ylabel("Wine (Per Capita)")
plt.tight_layout()
plt.figure(figsize = (15,5))

plt.savefig("Figures/top_andBottom_ten_wine.png")
plt.show
```

## Conclusion

### Discussion
  * Finding a relationship between happiness and drinking consumption is more complicated than we initially thought.
  * We needed to find what makes a country happier than others; and that involves analyzing social support, views of corruption, GDP, and freedom of life choices for each country and region

### Findings
  * Strong correlation between a a countries’ happiness and GDP
  * Countries with higher social support, freedom of choices, and less views of corruption tend to have higher GDP and happiness scores
  * The region where a person lives also impacts a person’s happiness.
  * Sub-Sahara Africa and Southeast Asia vs. Western Europe and Australia and New Zealand

### Final Thoughts
  * Initially we thought the less happy a country was, the higher drinking consumption they would have.  However we found the opposite.
  * Is it because poor countries have less to spend or something else? More research is needed.