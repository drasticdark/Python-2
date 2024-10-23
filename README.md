import pandas as pd

# Load the dataset
def load_data(file_path):
    return pd.read_csv(file_path)

def demographic_data_analysis(df):
    # How many people of each race are represented in this dataset?
    race_count = df['race'].value_counts()

    # What is the average age of men?
    average_age_men = df[df['sex'] == 'Male']['age'].mean()

    # What is the percentage of people who have a Bachelor's degree?
    percentage_bachelors = (df['education'] == 'Bachelors').mean() * 100

    # What percentage of people with advanced education (Bachelors, Masters, or Doctorate) make more than 50K?
    advanced_education = df['education'].isin(['Bachelors', 'Masters', 'Doctorate'])
    percentage_high_earning_advanced = (df[advanced_education]['salary'] == '>50K').mean() * 100

    # What percentage of people without advanced education make more than 50K?
    non_advanced_education = ~advanced_education
    percentage_high_earning_non_advanced = (df[non_advanced_education]['salary'] == '>50K').mean() * 100

    # What is the minimum number of hours a person works per week?
    min_hours_per_week = df['hours-per-week'].min()

    # What percentage of the people who work the minimum number of hours per week have a salary of more than 50K?
    percentage_high_earning_min_hours = (df[df['hours-per-week'] == min_hours_per_week]['salary'] == '>50K').mean() * 100

    # What country has the highest percentage of people that earn >50K and what is that percentage?
    country_earning_high = df[df['salary'] == '>50K'].groupby('native-country').size()
    country_total = df['native-country'].value_counts()
    country_percentage = (country_earning_high / country_total * 100).fillna(0)
    highest_percentage_country = country_percentage.idxmax()
    highest_percentage_value = country_percentage.max()

    # Identify the most popular occupation for those who earn >50K in India.
    popular_occupation_india = df[(df['native-country'] == 'India') & (df['salary'] == '>50K')]['occupation'].mode()[0]

    return {
        'race_count': race_count,
        'average_age_men': average_age_men,
        'percentage_bachelors': round(percentage_bachelors, 1),
        'percentage_high_earning_advanced': round(percentage_high_earning_advanced, 1),
        'percentage_high_earning_non_advanced': round(percentage_high_earning_non_advanced, 1),
        'min_hours_per_week': min_hours_per_week,
        'percentage_high_earning_min_hours': round(percentage_high_earning_min_hours, 1),
        'highest_percentage_country': highest_percentage_country,
        'highest_percentage_value': round(highest_percentage_value, 1),
        'popular_occupation_india': popular_occupation_india
    }

if __name__ == "__main__":
    # Example usage:
    df = load_data('path/to/your/dataset.csv')  # Update with your dataset path
    results = demographic_data_analysis(df)
    print(results)
