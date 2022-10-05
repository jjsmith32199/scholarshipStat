# scholarshipStat
import pandas as pd
import matplotlib as plot
from matplotlib import pyplot as plt


scholarship = pd.read_csv("C:\\Users\\Justin\\Documents\\Python Project\\NY-Scholarships.csv")

# Let's check to make sure there are no null values

print(scholarship.isnull().sum())

# This dataset is clear of null values
# If null values HAD been found,
# I believe the best course for this dataset would be to drop them using the following line of code:
# scholarship.dropna(inplace=True)

# We want to convert a couple of the columns into string values as they are specific numeric
# codes that will not need to be accounted for when calculating for averages later on
# These will be to College code and Federal School Code columns
scholarship['TAP College Code'] = scholarship['TAP College Code'].astype(str)
scholarship['Federal School Code'] = scholarship['Federal School Code'].astype(str)

# After inputting the code to convert them, I can print out the data types using the following
print(scholarship.dtypes)


# This dataset is organized chronologically by year,
# so if we pull the bottom 10 entries and the first 10 entries,
# we can get an idea how the most current year and the earliest year compare
print(scholarship.head(5))
print(scholarship.tail(5))

#Using the head and tail functions do not give us enough depth, graphing will take us a lot further.

# This line plot will visualize for us the total scholarship dollars granted per recorded year between 2009-2018
print(scholarship[['Scholarship Dollars', 'Academic Year']].groupby('Academic Year').sum().round(2))
dollarTotals = {'Academic Year': [2009, 2010, 2011, 2012, 2013, 2014, 2015, 2016, 2017, 2018],
                'Scholarship Fund Totals': [33964596.66, 31804986.16, 33554125.43, 32646421.85, 31312722.12,
                                            35051057.91, 39702332.86, 44473551.44, 50689267.51, 54150138.97]
                }
scholarTotal = pd.DataFrame(data=dollarTotals)


scholarTotal.plot.line(x='Academic Year', y='Scholarship Fund Totals', rot=60,
                       title='TAP Scholarship Dollars per Year(In millions)', color='black')
plt.grid(True, linewidth=0.5, linestyle='--')
plt.legend('')
plt.show()



# We want to find the average Scholarship Dollars given out by the TAP per year using the following code
# This code will also round to 2 decimal points to make it a recognizable monetary value
print(scholarship[['Scholarship Dollars', 'Academic Year']].groupby('Academic Year').mean().round(2))
# We'll use the results to create a dictionary that allows us to plot it on a bar graph for simplicity of viewing
dollarAvg = {'Academic Year': [2009, 2010, 2011, 2012, 2013, 2014, 2015, 2016, 2017, 2018],
             'Scholarship Dollars': [101085.11, 96087.57, 102612.00, 95737.31, 103684.51, 123419.22, 141794.05,
                                     154960.11, 187045.27, 188676.44]
             }

scholarAvg = pd.DataFrame(data=dollarAvg)

scholarAvg.plot.bar(x='Academic Year', y='Scholarship Dollars', rot=60,
                    title='Average Scholarship Dollars Granted Per Year',
                    color=['darkseagreen', 'lightgreen', 'lime', 'springgreen', 'lawngreen', 'seagreen',
                            'forestgreen', 'darkolivegreen', 'g', 'darkgreen'])

plt.grid(True, linewidth=0.5, linestyle='--')
plt.show()

# The final visualization will give an idea of what the average cost per Sector is.
# Average dollars granted with the headcount of each sector

scholarship.groupby('TAP Sector Group')['Scholarship Dollars'].mean(numeric_only=True).round(2)

scholarship[['Scholarship Headcount', 'TAP Sector Group']].groupby('TAP Sector Group').sum()


sectorPop = {'Student Count': [27131, 2739, 69097, 12536, 98899, 2860, 2, 37, 417],
             'Scholarship Dollars': [166113.25, 54996.94, 300484.29, 80438.80, 114809.03, 23618.55,  487.00, 2593.92, 9381.35]
             }
index = ['CUNY SR', 'CUNY CC', 'SUNY SO', 'SUNY CC', 'INDEPENDENT', 'BUS. DEGREE', 'BUS. NON-DEG', 'OTHER', 'VET SCHOOL']

sectorTot = pd.DataFrame(data=sectorPop, index=index)


sectorTot.plot.bar(rot=15, title='Sector Headcount & Avg Dollars Spent', color=['red', 'blue'],
                   edgecolor='black')
plt.grid(True, linewidth=0.5, linestyle='--')
plt.legend()
plt.xlabel('Sector Name')
plt.show(block=True)
