# webscrap-gomechanic
A python web scrapping code to create a csv file with price list for all vehicles on gomechanic.in

## Video demo (Click Image)
[![](https://raw.githubusercontent.com/avialxee/webscrap-gomechanic/master/screenshot_88.png)](http://www.youtube.com/watch?v=y_tYIVVmCSM "")

## Code
```

##GLOBAL VALUES
link = []
data = []
price = []

    

for v in open('all_links.csv') :
    link.append(v.split(','))
for d in open('vehicles.csv') :
    data.append(d.split(','))

#DASHER FUNCTION FOR WEBSITE CALLS
def makedash(variable) :
    str2 = ""
    for i in range(len(variable)):
        if(variable[i] == ' '):
            str2 = str2 + '-'
        else:
            str2 = str2 + variable[i]
    return(str2)


## PriceFetcher
def GetPriceandappend(Br,Mo,Bp,Sp,Cp) :
    new = ((Br,Mo,Bp,Sp,Cp))
    price.append(new)
    df = pd.DataFrame(price, columns=['Brand','Model','Basic_Price','Standard_Price','Comprehensive_Price'])
                           
    df.to_csv('Final_Price_quotes.csv')
    print(Mo, "saved")



## MAIN FUNCTION
def Pricequotation () :
    
    driver_path = "D:/Intelligence/python/server-side/chromedriver.exe"
    brave_path = "C:/Program Files (x86)/BraveSoftware/Brave-Browser/Application/brave.exe"
    for i in range(len(data)) :
        mname = data[i][2]
        model = makedash(data[i][2]).strip()
        Brand = data[i][1]
        fuel = data[i][3]
        modelt = 'model'
        for j in range(len(link)) :
            linker = str(link[j][1])
            if (model in linker and model!= modelt) :
                modelt = model
                option = webdriver.ChromeOptions()
                option.binary_location = brave_path
                browser = webdriver.Chrome(executable_path=driver_path, chrome_options=option)
                browser.get(linker)
                print(" car =", model)

            
                try:
                    
                    dropdown = browser.find_element_by_xpath('/html/body/div[6]/div/div/div/div/form/div[1]/div[2]/select')
                    select1 = Select(dropdown)
                    select1.select_by_visible_text(Brand)
                    time.sleep(2)
                    
                    dropdown2 = browser.find_element_by_xpath('/html/body/div[6]/div/div/div/div/form/div[1]/div[3]/select')
                    select2 = Select(dropdown2)
                    select2.select_by_visible_text(mname)
                    time.sleep(1)
                    dropdown3 = browser.find_element_by_xpath('/html/body/div[6]/div/div/div/div/form/div[1]/div[4]/select/option[2]')
                    dropdown3.click()
                    #select3 = Select(dropdown3)
                    #select3.select_by_visible_text(fuel)
                    print("passed fuel peacefully")
                    time.sleep(2)
                    
                    button = browser.find_element_by_id('0-submit-popUp')
                    button.click()
                    time.sleep(2)
                    BasicPrice = browser.find_element_by_xpath('/html/body/div[6]/div[4]/div[6]/div[1]/div/div/div/div[1]/a/div[2]/div/p[1]').text
                    StandarPrice = browser.find_element_by_xpath('/html/body/div[6]/div[4]/div[6]/div[1]/div/div/div/div[3]/a/div[2]/div/p[1]').text
                    ComprehensivePrice = browser.find_element_by_xpath('/html/body/div[6]/div[4]/div[6]/div[1]/div/div/div/div[5]/a/div[2]/div/p[1]').text
                    GetPriceandappend(Brand, model, BasicPrice, StandarPrice, ComprehensivePrice)
                    browser.close()
                
                except : break
                           
   
Pricequotation ()


```
