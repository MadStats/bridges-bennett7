###  Bridges of the USA.  
[I think this data is so interesting.](https://www.fhwa.dot.gov/bridge/nbi/ascii.cfm)  When you find some data, you should always look for the [data descriptions](https://www.fhwa.dot.gov/bridge/mtguide.pdf).  From page 38, we have the ratings of bridge conditions (for Deck, Superstructure, and Substructure), for *every bridge in the US*, for every year back to 1991.  Lots of other data on each bridge (e.g. county FIPS code of the bridge). 

Warm up:  make a file with bridge ID, year, fips codes, condition ratings, and a few other variables that interest you.  Make your code reproducible and post to github.  

library(data.table)
Alaska = fread('https://www.fhwa.dot.gov/bridge/nbi/2014/delimited/AK14.txt')
#make for loop to pick out each and every state here from this one year
#Use data scraping to read all the url's into the system.
#Will have to make unique name for them all?
Alaska = as.data.frame(Alaska)
#Make the data usable and reusable
#Remove all the unnecessary columns in the data(same for all sets)
#Columns in use are numbers: 1,2,9,11,13,20,21,24,27,73
Alaska = Alaska[,-c(3:8,10,12,14:19,22,23,25,26,28:72,74:134)]
#Rename the columns for ease of use
Alaska = rename(Alaska, c("STATE_CODE_001" ="STATE", "STRUCTURE_NUMBER_008" = "BRIDGE NUM", "COUNTY_CODE_003" = "COUNTY", "FEATURES_DESC_006A" = "LOCATION", "FACILITY_CARRIED_007" = "ROAD TYPE", "LAT_016" = "LATITUDE", "LONG_017" = "LONGITUDE", "MAINTENANCE_021" = "CONDITION", "YEAR_BUILT_027" = "YEAR", "OPERATING_RATING_064" = "LIFE"))
#Set the state code to be a factor for use of concat
Alaska$State = as.factor(Alaska$STATE)
#For all the first 9 states, append a 0 in front of the state number for creation of fips codes
Alaska$append = "0"
Alaska$temp = paste(Alaska$append, Alaska$STATE, sep = "")
#Create fips code based off state and county numbers
Alaska$FIPS = paste(Alaska$temp, Alaska$COUNTY, sep = "")


#Future code, create a data scraping system to read each state into its own file, done by the final part of the url which contains the state abbreviation and the year. Using this, create a file for each state, with an additional column that contains the year. Once each file is created, see if possible to assimilate those 50 files into one, separated by the state names and years. 