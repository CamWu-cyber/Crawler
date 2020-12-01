# 合并之前
任务：针对dandanzan.com，gimy.com, ifvod.com, ooe.com, wekan.com五个侵权网站，提取每部电视剧每一集的url和title，结果写入excel中，excel的名字为当天的日期.

思路：每个网站单独分析，采用selenium+chromedriver去爬取数据。具体代码如下：

#### 1. dandanzan.py
  
    from selenium import webdriver
    import xlsxwriter
    import time

    driver = webdriver.Chrome('C:\\Users\\user\\Desktop\\AU\\chromedriver.exe')
    #driver.maximize_window()
    nextfather = "https://www.dandanzan.com/dianshiju/-%E9%A6%99%E6%B8%AF-2020--.html"
    titles = []
    urls = []

    while 1:
        driver.get(nextfather)
        links = driver.find_elements_by_xpath("//div[@class='lists-content']/ul/li/h2/a")
        fatherurls = []
        for link in links:
            fatherurls.append(link.get_attribute('href'))

        file = open('C:\\Users\\user\\Desktop\\AU\\dandanzan\\error lists.txt', 'r', encoding='utf-8')
        errorlines = file.readlines()
        truesites = []
        for i in range(len(fatherurls)):
            boo = True
            for j in range(len(errorlines)):
                if errorlines[j].strip() in str(fatherurls[i]):
                    boo = False
                else:
                    continue
            if boo:
                truesites.append(fatherurls[i])
            else:
                continue
        #print(truesites)

        episodes = []
        for truesite in truesites:
            episodes.clear()
            #print(episodes)
            driver.get(truesite)
            time.sleep(10)
            titles.append(driver.title)          #father's title
            urls.append(driver.current_url)      #father's url

        #判断是否有下一页
        try:
            driver.get(nextfather)
            time.sleep(5)
            driver.find_element_by_xpath("//ul/li[@class='next-page']/a").click()
            nextfather = driver.current_url
            fatherurls.clear()
            truesites.clear()
        except:
            print("已经没有下一页了！")
            break

    everyday = time.strftime("%d%m%Y", time.localtime())
    endfile='C:\\Users\\user\\Desktop\\AU\\dandanzan\\'+everyday+'.xlsx'
    workbook = xlsxwriter.Workbook(endfile)
    worksheet = workbook.add_worksheet('Sheet1')
    keyword = '蛋蛋赞影院'
    worksheet.write(0, 0, 'Date')
    worksheet.write(0, 1, 'Keywords')
    worksheet.write(0, 2, 'title')
    worksheet.write(0, 3, 'url')
    for i in range(1, len(titles)+1):
        worksheet.write(i, 0, time.strftime("%d/%m/%Y %H:%M %p", time.localtime()))
        worksheet.write(i, 1, keyword)
        worksheet.write(i, 2, titles[i-1])
        worksheet.write(i, 3, urls[i-1])

    workbook.close()

    #关闭浏览器
    driver.quit()

#### 2.gimy.py
    from selenium import webdriver
    import xlsxwriter
    import time

    driver = webdriver.Chrome('C:\\Users\\user\\Desktop\\AU\\chromedriver.exe')
    #driver.maximize_window()
    nextfather = "https://gimy.co/cat/2-%E9%A6%99%E6%B8%AF-------1---2020.html"
    titles = []
    urls = []

    while 1:
        driver.get(nextfather)
        links = driver.find_elements_by_xpath("//h5[@class='text-overflow']/a")
        fatherurls = []
        for link in links:
            fatherurls.append(link.get_attribute('href'))

        file = open('C:\\Users\\user\\Desktop\\AU\\gimy\\error lists.txt', 'r', encoding='utf-8')
        errorlines = file.readlines()
        truesites = []
        for i in range(len(fatherurls)):
            boo = True
            for j in range(len(errorlines)):
                if errorlines[j].strip() in str(fatherurls[i]):
                    boo = False
                else:
                    continue
            if boo:
                truesites.append(fatherurls[i])
            else:
                continue
        #print(truesites)

        episodes = []
        for truesite in truesites:
            episodes.clear()
            #print(episodes)
            driver.get(truesite)
            time.sleep(5)
            titles.append(driver.title)          #father's title
            urls.append(driver.current_url)      #father's url
            links = driver.find_elements_by_xpath("//div[@class='playlist']/ul/li/a")
            for link in links:
                episodes.append(link.get_attribute('href'))
            #print(episodes)
            for episode in episodes:
                driver.get(episode)
                time.sleep(5)
                titles.append(driver.title)      #child's title
                urls.append(driver.current_url)  #child's url

        #判断是否有下一页
        try:
            driver.get(nextfather)
            time.sleep(5)
            driver.find_element_by_xpath("//ul/li/a[@class='next pagegbk']").click()
            nextfather = driver.current_url
            fatherurls.clear()
            truesites.clear()
        except:
            print("已经没有下一页了！")
            break

    everyday = time.strftime("%d%m%Y", time.localtime())
    endfile='C:\\Users\\user\\Desktop\\AU\\gimy\\'+everyday+'.xlsx'
    workbook = xlsxwriter.Workbook(endfile)
    worksheet = workbook.add_worksheet('Sheet1')
    keyword = 'Gimy TV'
    worksheet.write(0, 0, 'Date')
    worksheet.write(0, 1, 'Keywords')
    worksheet.write(0, 2, 'title')
    worksheet.write(0, 3, 'url')
    for i in range(1, len(titles)+1):
        worksheet.write(i, 0, time.strftime("%d/%m/%Y %H:%M %p", time.localtime()))
        worksheet.write(i, 1, keyword)
        worksheet.write(i, 2, titles[i-1])
        worksheet.write(i, 3, urls[i-1])

    workbook.close()

    #关闭浏览器
    driver.quit()

#### 3.ifvod.py
    from selenium import webdriver
    from selenium.common.exceptions import NoSuchElementException
    import xlsxwriter
    import time

    driver = webdriver.Chrome('C:\\Users\\user\\Desktop\\AU\\chromedriver.exe')
    #driver.maximize_window()
    nextfather = "https://www.ifvod.tv/list?keyword=&star=&page=1&pageSize=30&cid=0,1,4,14&year=%E4%BB%8A%E5%B9%B4&language=-1&region=%E6%B8%AF%E5%8F%B0&status=-1&orderBy=0&desc=true"
    titles = []
    urls = []

    while 1:
        driver.get(nextfather)
        time.sleep(5)
        links = driver.find_elements_by_xpath("//div[@class='search-results d-flex flex-wrap justify-content-between ng-star-inserted']/app-video-teaser/div/a")
        fatherurls = []
        for link in links:
            fatherurls.append(link.get_attribute('href'))

        file = open('C:\\Users\\user\\Desktop\\AU\\ifvod\\error lists.txt', 'r', encoding='utf-8')
        errorlines = file.readlines()
        truesites = []
        for i in range(len(fatherurls)):
            boo = True
            for j in range(len(errorlines)):
                if errorlines[j].strip() in str(fatherurls[i]):
                    boo = False
                else:
                    continue
            if boo:
                truesites.append(fatherurls[i])
            else:
                continue
        print(truesites)

        episodes = []
        for truesite in truesites:
            episodes.clear()
            driver.get(truesite)
            time.sleep(5)
            titles.append(driver.title)          #father's title
            urls.append(driver.current_url)      #father's url
            try:
                driver.find_element_by_xpath("//a[@class='expander media-button text-small mr-2']").click()
                time.sleep(2)
            except NoSuchElementException:
                pass
            links = driver.find_elements_by_xpath("//div[@class='d-flex flex-wrap my-1 text-small ng-star-inserted']/div/app-media-button/div/a")
            for link in links:
                episodes.append(link.get_attribute('href'))
            print(episodes)
            for episode in episodes:
                if 'javascript:void(0);' in episode:
                    continue
                else:
                    driver.get(episode)
                    #time.sleep(5)
                    titles.append(driver.title)      #child's title
                    urls.append(driver.current_url)  #child's url

        #判断是否有下一页
        driver.get(nextfather)
        time.sleep(5)
        if not driver.find_element_by_xpath("//app-pager[@class='ng-star-inserted']/ul/li[5]").get_attribute('class'):
            driver.find_element_by_xpath("//app-pager[@class='ng-star-inserted']/ul/li[5]/a").click()
            nextfather = driver.current_url
            fatherurls.clear()
            truesites.clear()
        else:
            print("已经没有下一页了！")
            break

    everyday = time.strftime("%d%m%Y", time.localtime())
    endfile='C:\\Users\\user\\Desktop\\AU\\ifvod\\'+everyday+'.xlsx'
    workbook = xlsxwriter.Workbook(endfile)
    worksheet = workbook.add_worksheet('Sheet1')
    keyword = '视频,视频分享,视频搜索,视频播放,视频社区'
    worksheet.write(0, 0, 'Date')
    worksheet.write(0, 1, 'Keywords')
    worksheet.write(0, 2, 'title')
    worksheet.write(0, 3, 'url')
    for i in range(1, len(titles)+1):
        worksheet.write(i, 0, time.strftime("%d/%m/%Y %H:%M %p", time.localtime()))
        worksheet.write(i, 1, keyword)
        worksheet.write(i, 2, titles[i-1])
        worksheet.write(i, 3, urls[i-1])

    workbook.close()

    #关闭浏览器
    driver.quit()

#### 4.ooe.py
    from selenium import webdriver
    import xlsxwriter
    import time

    driver = webdriver.Chrome('C:\\Users\\user\\Desktop\\AU\\chromedriver.exe')
    #driver.maximize_window()
    nextfather = "https://www.ooe.la/vodshow/2-%E9%A6%99%E6%B8%AF----------2020/"
    titles = []
    urls = []
    count = 5

    while 1:
        driver.get(nextfather)
        links = driver.find_elements_by_xpath("//ul[@class='mlist']/li/a")
        fatherurls = []
        for link in links:
            fatherurls.append(link.get_attribute('href'))

        file = open('C:\\Users\\user\\Desktop\\AU\\ooe\\error lists.txt', 'r', encoding='utf-8')
        errorlines = file.readlines()
        truesites = []
        for i in range(len(fatherurls)):
            boo = True
            for j in range(len(errorlines)):
                if errorlines[j].strip() in str(fatherurls[i]):
                    boo = False
                else:
                    continue
            if boo:
                truesites.append(fatherurls[i])
            else:
                continue
        print(truesites)

        episodes = []
        for truesite in truesites:
            episodes.clear()
            #print(episodes)
            driver.get(truesite)
            time.sleep(5)
            titles.append(driver.title)          #father's title
            urls.append(driver.current_url)      #father's url
            links = driver.find_elements_by_xpath("//div[@class='play-list']/a")
            for link in links:
                episodes.append(link.get_attribute('href'))
            #print(episodes)
            for episode in episodes:
                driver.get(episode)
                time.sleep(5)
                titles.append(driver.title)      #child's title
                urls.append(driver.current_url)  #child's url

        #判断是否有下一页

        if count:
            driver.get(nextfather)
            time.sleep(5)
            nextfather = driver.find_element_by_xpath("//div[@class='page_info']/a[8]").get_attribute('href')
            fatherurls.clear()
            truesites.clear()
            count -= 1
            continue
        else:
            print("已经没有下一页了！")
            break

    everyday = time.strftime("%d%m%Y", time.localtime())
    endfile='C:\\Users\\user\\Desktop\\AU\\ooe\\'+everyday+'.xlsx'
    workbook = xlsxwriter.Workbook(endfile)
    worksheet = workbook.add_worksheet('Sheet1')
    keyword = '南瓜,南瓜电影,南瓜影院,电影下载,免费电影下载,迅雷电影下载,最新电影'
    worksheet.write(0, 0, 'Date')
    worksheet.write(0, 1, 'Keywords')
    worksheet.write(0, 2, 'title')
    worksheet.write(0, 3, 'url')
    for i in range(1, len(titles)+1):
        worksheet.write(i, 0, time.strftime("%d/%m/%Y %H:%M %p", time.localtime()))
        worksheet.write(i, 1, keyword)
        worksheet.write(i, 2, titles[i-1])
        worksheet.write(i, 3, urls[i-1])

    workbook.close()

    #关闭浏览器
    driver.quit()

#### 5.wekan.py
    from selenium import webdriver
    import xlsxwriter
    import time

    driver = webdriver.Chrome('C:\\Users\\user\\Desktop\\AU\\chromedriver.exe')
    #driver.maximize_window()
    nextfather = "https://www.wekan.tv/filter-tvdrama-10037-2020-0-hot"
    titles = []
    urls = []

    driver.get(nextfather)
    time.sleep(10)
    links = driver.find_elements_by_xpath("//ul[@class='poster-layout']/li/div/div/a")
    fatherurls = []
    for link in links:
        fatherurls.append(link.get_attribute('href'))

    file = open('C:\\Users\\user\\Desktop\\AU\\wekan\\error lists.txt', 'r', encoding='utf-8')
    errorlines = file.readlines()

    truesites = []
    for i in range(len(fatherurls)):
        boo = True
        for j in range(len(errorlines)):
            if errorlines[j].strip() == str(fatherurls[i]):
                boo = False
            else:
                continue
        if boo:
            truesites.append(fatherurls[i])
        else:
            continue

    episodes = []
    for truesite in truesites:
        episodes.clear()
        driver.get(truesite)
        time.sleep(5)
        links = driver.find_elements_by_xpath("//ul[@class='select-part__part-list']/li/div/a")
        for link in links:
            episodes.append(link.get_attribute('href'))
        #print(episodes)
        for episode in episodes:
            driver.get(episode)
            time.sleep(5)
            titles.append(driver.title)      #child's title
            urls.append(driver.current_url)  #child's url

    print("已经没有下一页了！")

    everyday = time.strftime("%d%m%Y", time.localtime())
    endfile='C:\\Users\\user\\Desktop\\AU\\wekan\\'+everyday+'.xlsx'
    workbook = xlsxwriter.Workbook(endfile)
    worksheet = workbook.add_worksheet('Sheet1')
    keyword = '看tv-Kantv-华人首家在线视频分享网站'
    worksheet.write(0, 0, 'Date')
    worksheet.write(0, 1, 'Keywords')
    worksheet.write(0, 2, 'title')
    worksheet.write(0, 3, 'url')
    for i in range(1, len(titles)+1):
        worksheet.write(i, 0, time.strftime("%d/%m/%Y %H:%M %p", time.localtime()))
        worksheet.write(i, 1, keyword)
        worksheet.write(i, 2, titles[i-1])
        worksheet.write(i, 3, urls[i-1])

    workbook.close()

    #关闭浏览器
    driver.quit()
