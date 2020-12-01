# 合并之前
任务：针对dandanzan.com，gimy.com, ifvod.com, ooe.com, wekan.com五个侵权网站，提取每部电视剧每一集的url和title，结果写入excel中，excel的名字为当天的日期.

思路：每个网站单独分析，采用selenium+chromedriver去爬取数据。具体代码如下：

####1. dandanzan.py
  
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
