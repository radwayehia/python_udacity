# python_udacity
from ast import Try
import time
import pandas as pd
import numpy as np

CITY_DATA = { 'chicago': 'chicago.csv',
              'new york city': 'new_york_city.csv',
              'washington': 'washington.csv' }

def get_filters(): 
    
    """
    Asks user to specify a city, month, and day to analyze.

    Returns:
        (str) city - name of the city to analyze
        (str) month - name of the month to filter by, or "all" to apply no month filter
        (str) day - name of the day of week to filter by, or "all" to apply no day filter
    """
    print('Hello! Let\'s explore some US bikeshare data!')
  # get user input for city (chicago, new york city, washington). HINT: Use a while loop to handle invalid inputs

    city = input("which city data do you want to access? \"chicago ,New york city or Washington\"? \n")
    Cities_options = ['new york city', 'washington', 'chicago']
    while city not in Cities_options :
        print("plz, choose from quoted options in question \"turn off capslk\"")
        city = input("which city data do you want to access?\"chicago ,New york city or Washington\"\n")
        

    # get user input for month (all, january, february, ... , june)
    month = input("In which month are you looking up ? \"jan, fab, mar, apr, may, june, july, august, sept, oct, nov, dec, or all\"? \n")
    Month_options = ['jan', 'fab', 'mar', 'apr','may', 'june', 'july', 'aug', 'sept', 'oct', 'nov', 'dec','all']
    while month not in Month_options:
        print("plz, choose from quoted options in question \"turn off capslk\"\n")
        month = input("In which month are you looking up ? \"jan, fab, mar, apr, may, june, july, august, sept, oct, nov, dec, or all?\" \n")
        

    # get user input for day of week (all, monday, tuesday, ... sunday)
    day = input("In which day are you looking up ? \"saturday, sunday, monday, tuesday, wednesday, thursday, friday saturday or all\"? \n")
    Days_options  = ['sunday', 'monday', 'tuesday', 'wednesday', 'thursday', 'friday', 'saturday','all']
    while day not in Days_options:
        day = input("In which day are you looking up ? \" sunday, monday, tuesday, wednesday, thursday ,friday, saturday or all?\"\n")
        

    print('-'*40)
    return city, month, day


def filtering(city, month, day):
    """
    Loads data for the specified city and filters by month and day if applicable.

    Args:
        (str) city - name of the city to analyze
        (str) month - name of the month to filter by, or "all" to apply no month filter
        (str) day - name of the day of week to filter by, or "all" to apply no day filter
    Returns:
        load_data - Pandas DataFrame containing city data filtered by month and day
    """
    if city == 'washington':
        load_data = pd.read_csv('washington.csv')
        load_data['Start Time'] = pd.to_datetime(load_data['Start Time'])
        load_data['month'] = load_data['Start Time'].dt.month
        load_data['day_of_week'] = load_data['Start Time'].dt.day_name()
        if month != 'all':
            months = ['jan', 'fab', 'mar', 'apr','may', 'june', 'july', 'aug', 'sept', 'oct', 'nov', 'dec']
            month = months.index(month) + 1
            load_data = load_data[load_data['month'] == month]
        if day != 'all':
                    load_data = load_data[load_data['day_of_week'] == day.title()]
    if city == 'new york city':
        load_data = pd.read_csv('new_york_city.csv')
        load_data['Start Time'] = pd.to_datetime(load_data['Start Time'])
        load_data['month'] = load_data['Start Time'].dt.month
        load_data['day_of_week'] = load_data['Start Time'].dt.day_name()
        if month != 'all':
            months = ['jan', 'fab', 'mar', 'apr','may', 'june', 'july', 'aug', 'sept', 'oct', 'nov', 'dec']
            month = months.index(month) + 1
            load_data = load_data[load_data['month'] == month]
        if day != 'all':
                    load_data = load_data[load_data['day_of_week'] == day.title()]
    if city == 'chicago':
        load_data = pd.read_csv('chicago.csv')
        load_data['Start Time'] = pd.to_datetime(load_data['Start Time'])
        load_data['month'] = load_data['Start Time'].dt.month
        load_data['day_of_week'] = load_data['Start Time'].dt.day_name()
        if month != 'all':
            months = ['jan', 'fab', 'mar', 'apr','may', 'june', 'july', 'aug', 'sept', 'oct', 'nov', 'dec']
            month = months.index(month) + 1
            load_data = load_data[load_data['month'] == month]
        if day != 'all':
                    load_data = load_data[load_data['day_of_week'] == day.title()]
    return load_data

def time_stats(load_data):
    """Displays statistics on the most frequent times of travel."""
    print('\nCalculating The Most Frequent Times of Travel...\n')
    start_time = time.time()

    # display the most common month
    load_data['Start Time'] = pd.to_datetime(load_data['Start Time'])
    load_data['month'] = load_data['Start Time'].dt.month
    popular_month = load_data['month'].mode()[0]
    print('the most common month: '+str(popular_month)) 
    # display the most common day of week

    load_data['Start Time'] = pd.to_datetime(load_data['Start Time'])
    load_data['day'] = load_data['Start Time'].dt.day
    popular_day = load_data['day'].mode()[0]
    print('the most common day: '+str(popular_day)) 

    # display the most common start hour
    load_data['Start Time'] = pd.to_datetime(load_data['Start Time'])
    load_data['hour'] = load_data['Start Time'].dt.hour
    popular_hour = load_data['hour'].mode()[0]
    print('the most common hour: '+str(popular_hour)) 


    print("\nThis took %s seconds." % (time.time() - start_time))
    print('-'*40)


def station_stats(load_data):
    """Displays statistics on the most popular stations and trip."""

    print('\nCalculating The Most Popular Stations and Trip...\n')
    start_time = time.time()

    # display most commonly used start station
    
    popular_start_station = load_data['Start Station'].mode()
    print('the most common start staion: '+str(popular_start_station)) 

    # display most commonly used end station
    popular_end_station = load_data['End Station'].mode()
    print('the most common end staion: '+str(popular_end_station)) 


    # display most frequent combination of start station and end station trip
    popular_comp_station = (load_data['Start Station']+","+ ['End Station']).mode()[0]
    print("the most popular combination: " + str(popular_comp_station.split(',')[0]))
    print("\nThis took %s seconds." % (time.time() - start_time))
    print('-'*40)


def trip_duration_stats(load_data):
    """Displays statistics on the total and average trip duration."""

    print('\nCalculating Trip Duration...\n')
    start_time = time.time()

    # display total travel time
    total_travel_time = load_data['Trip Duration'].sum()
    print('total travel time: '+str(total_travel_time))
    # display mean travel time
    
    mean_travel_time = load_data['Trip Duration'].mean()
    print("the average travel time: "+str(mean_travel_time))

    print("\nThis took %s seconds." % (time.time() - start_time))
    print('-'*40)


def user_stats(load_data):
    """Displays statistics on bikeshare users."""

    print('\nCalculating User Stats...\n')
    start_time = time.time()

    # Display counts of user types

    user_type_count = load_data['User Type'].unique()
    print("the user types count: "+str(len(user_type_count)))
    # Display counts of gender
    try:
        gender_counts = load_data['Gender'].value_counts()
        print("male: "+str(gender_counts[0]),"female: "+str(gender_counts[1]))
    except:
        print()

    # Display earliest, most recent, and most common year of birth
    
    try:
        most_common_birth = load_data['Birth Year'].mode()
        print("the most common birth year: " + str(most_common_birth[0]))
        # load_data['Birth Year'] = pd.to_datetime(load_data['Birth Year'])
        # load_data['year'] = load_data['Birth Year'].dt.year
        recent_year = load_data['Birth Year'].max()
        print('the most recent year: '+str(recent_year)) 
        earlist_year = load_data['Birth Year'].min()
        print('the earlist year: '+str(earlist_year)) 
    except:
        print()

    print("\nThis took %s seconds." % (time.time() - start_time))
    print('-'*40)

def five_rows(load_data,passing):
    header = load_data.head(passing)
    print(header)
def main():
    passing = 0
    while True:
        city, month, day = get_filters()
        load_data = filtering(city, month, day)

        time_stats(load_data)
        station_stats(load_data)
        trip_duration_stats(load_data)
        user_stats(load_data)
        while True:
            breaker = input("do you want to view the first five row of information?")
            if breaker.lower() != "yes":
                break
            passing += 5
            five_rows(load_data,passing)

        restart = input('\nWould you like to restart? Enter yes or no.\n')
        if restart.lower() != 'yes':
            break


if __name__ == "__main__":
	main()
