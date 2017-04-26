---
layout: post
title: "爬一爬教务系统"
category: "python"
keywords: ["python", "urllib"]
description: "python urllib"
tags: ["python", "urllib"]
---
{%highlight python%}
# coding:utf-8

import urllib
import urllib2
import cookielib
import getpass


class tust_login(object):


    def __init__(self):
        '''
        一些url：
        全部及格成绩："http://jwxt.tust.edu.cn/gradeLnAllAction.do?type=ln&oper=qb"
        不及格成绩："http://jwxt.tust.edu.cn/gradeLnAllAction.do?type=ln&oper=bjg"
        本学期成绩："http://jwxt.tust.edu.cn/bxqcjcxAction.do"
        本学期课表：""http://jwxt.tust.edu.cn/syglSyxkAction.do?&oper=xsxkKcbAll""
        '''
        self.login_url = "http://jwxt.tust.edu.cn/newLoginAction.do"

        self.header = {
            'Accept':'text/html, application/xhtml+xml, image/jxr, */*',
            'Accept-Encoding':'gzip, deflate',
            'Accept-Language':'zh-Hans-CN, zh-Hans; q=0.5',
            'Cache-Control':'no-cache',
            'Connection':'Keep-Alive',
            'Content-Length':'79',
            'Content-Type':'application/x-www-form-urlencoded',
            'Cookie':'JSESSIONID=gbh5n65xWICvQ6ztgvWHv',
            'Host':'jwxt.tust.edu.cn',
            'Referer':'http://jwxt.tust.edu.cn/',
            'User-Agent':'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/51.0.2704.79 Safari/537.36 Edge/14.14393'
        }
        """创建一个带cookie的opener"""
        cj = cookielib.CookieJar()
        self.opener = urllib2.build_opener(urllib2.HTTPCookieProcessor(cj));



    def login(self):
        post_data = {
            'b':'0',
            'dzslh':'',
            'eflag':'',
            'evalue':'',
            'fs':'',
            'lx':'',
            'tips':'',
            'zjh1':'',
            'IDToken1':'username',
            'IDToken2':'password'
        }
        #post_data["IDToken1"] = raw_input("Input num:\n")
        #post_data["IDToken2"] = getpass.getpass("Input passwd:\n")
        post_data = urllib.urlencode(post_data)
        #request = urllib2.Request(self.login_url,post_data,self.header)
        #resp = self.opener.open(request)
        resp = self.opener.open(self.login_url,post_data)
        status = resp.getcode()
        if status == 200:
            print "login sussecs!"
            html = resp.read()
            html = unicode(html,"GBK").encode('UTF-8')
            print html
            #得到cookie后，再get请求
            print "A.全部及格成绩"
            print "B.不及格成绩"
            print "C.本学期成绩"
            print "D.本学期课表"
            option = raw_input("Please input option:")
            if option == 'A' or option == 'a' or option == '1':
                resp = self.opener.open("http://jwxt.tust.edu.cn/gradeLnAllAction.do?type=ln&oper=qb")
                html = resp.read()
                html = unicode(html,"GBK").encode('UTF-8')
                print html
            elif option == 'B' or option == 'b' or option == '2':
                resp = self.opener.open("http://jwxt.tust.edu.cn/gradeLnAllAction.do?type=ln&oper=bjg")
                html = resp.read()
                html = unicode(html,"GBK").encode('UTF-8')
                print html
            elif option == 'C' or option == 'c' or option == '3':
                resp = self.opener.open("http://jwxt.tust.edu.cn/bxqcjcxAction.do")
                html = resp.read()
                html = unicode(html,"GBK").encode('UTF-8')
                print html
            elif option == 'D' or option == 'd' or option == '4':
                resp = self.opener.open("http://jwxt.tust.edu.cn/syglSyxkAction.do?&oper=xsxkKcbAll")
                html = resp.read()
                html = unicode(html,"GBK").encode('UTF-8')
                print html
            else:
                print "input error!"

        else:
            print "login failed!"



if __name__ == "__main__":
    tust_login().login()

{%endhighlight%}
