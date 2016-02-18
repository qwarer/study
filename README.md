#__author__ = 'wd'
#coding=utf-8
#version 1.0
import time
import sys, urllib
import re
import os
import  requests
from  BeautifulSoup import  BeautifulSoup
import sys
reload(sys)
url="http://m.sohu.com/"

def getHtml(url):
    page = urllib.urlopen(url)
    content = page.read()
    return content

def getImg(content,dir):
    reg = r'src="(.+?\.jpg)"'
    imgre = re.compile(reg)
    imglist = re.findall(imgre,content)
    x = 0
    dir=dir+'\\img\\'
    os.makedirs(dir)
    for imgurl in imglist:
        urllib.urlretrieve(imgurl,dir+'%s.jpg' % x,)
        x+=1

def getCss(content,dir):
    reg = r'href="(.+?\.css)"'
    cssre = re.compile(reg)
    csslist = re.findall(cssre,content)
    x = 0
    dir=dir+'\\CSS\\'
    os.makedirs(dir)
    for css in csslist:
        cssPage=urllib.urlopen(css)
        cssContent=cssPage.read()
        CssFile=open(dir+'%s.css'%x,'w')
        x+=1
        CssFile.write(cssContent)
        CssFile.close()

def getJs(content,dir):
    reg = r'(?:<script(.*)>)(.*?)(?:</script>)'
    jsre = re.compile(reg)
    jslist = re.findall(jsre,content)
    print jslist
    x = 0
    dir=dir+'\\JS\\'
    os.makedirs(dir)
    for js in jslist:
        JsFile=open(dir+'%s.js'%x,'w')
        x+=1
        JsFile.write(str(js))
        JsFile.close()
def main():
    dir='d:\\'+str(time.strftime("%Y%m%d%H%M%S", time.localtime()))
    os.makedirs(dir)
    HtmlFileName=dir+'\\'+'index.html'
    content=getHtml(url)
    HtmlContent=open(HtmlFileName,'w')
    HtmlContent.write(content) #写入数据
    HtmlContent.close() #关闭文件
    getImg(content,dir)
    getCss(content,dir)
    getJs(content,dir)

if __name__ == '__main__':
    main()
