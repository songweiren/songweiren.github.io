---
layout: post
title:  "Python GUI之Tkinter入门模板"
date:   2020-07-07 22:49:52 +0800
categories: Python
tags: Python
---
```Python
import tkinter as tk
from tkinter import ttk
from WebHelp import *
import collections
from threading import Thread
import time
from htmlbs import *
 
 
class MainWindows(tk.Tk):
 
    TREESIZE = 30
    QUEDELAY = 30
    b_continueget = True
    __wh = ''
    __hb = HtmlBs()
    time_cost = 0.0
    index_list = []
    num_dict = collections.OrderedDict()
    __w = 1200
    __h = 350
 
    def __init__(self, path, url):
        super().__init__()  # 初始化基类
        self.title("夺金120S")
        self.geometry('1200x400')
        self.resizable(0, 0)
        self.ini_ui()
        self.__wh = WebHelp(path, url)
        self.__wh.configWeb()
        th_get = Thread(target=self.data_loop)
        th_get.setDaemon(True)
        th_get.start()
 
    def ini_ui(self):
        frame = tk.Frame(self, width=self.__w, height=self.__h)
        self.tree = ttk.Treeview(frame, show="headings")  # 表格
        tree_cln = ("期号", "中奖号码", "01,02", "01,03", "01,04", "01,05", "01,06", "01,07", "01,08", "01,09", "01,10", "01,11",
                                        "02,03", "02,04", "02,05", "02,06", "02,07", "02,08", "02,09", "02,10", "02,11",
                                        "03,04", "03,05", "03,06", "03,07", "03,08", "03,09", "03,10", "03,11",
                                        "04,05", "04,06", "04,07", "04,08", "04,09", "04,10", "04,11",
                                        "05,06", "05,07", "05,08", "05,09", "05,10", "05,11",
                                        "06,07", "06,08", "06,09", "06,10", "06,11",
                                        "07,08", "07,09", "07,10", "07,11",
                                        "08,09", "08,10", "08,11",
                                        "09,10", "09,11",
                                        "10,11"
        )
        self.tree["columns"] = tree_cln  # ("期号", "中奖号码", "01,02", "01,03", "01,04", "01,05", "01,06", "01,07", "01,08", "01,09", "01,10", "01,11")
        # 表示列,不显示
        for item in tree_cln:
            self.tree.column(item, width=60, anchor="center")
        self.tree.column("期号", width=80, anchor="center")
        self.tree.column("中奖号码", width=150, anchor="center")
        #  显示表头
        for item in tree_cln:
            self.tree.heading(item, text=item)
 
        self.tree.place(relx=0.004, rely=0.028, relwidth=0.964, relheight=0.95)
        # 给treeview添加滚动条
        self.VScroll1 = tk.Scrollbar(frame, orient='vertical', command=self.tree.yview)
        self.VScroll1.place(relx=0.971, rely=0.028, relwidth=0.014, relheight=0.958)
        self.VScroll2 = tk.Scrollbar(frame, orient='horizontal', command=self.tree.xview)
        self.VScroll2.place(relx=0.008, rely=0.961, relwidth=0.958, relheight=0.024)
        self.tree.configure(yscrollcommand=self.VScroll1.set, xscrollcommand=self.VScroll2.set)
        # 给tree view填充初始数据
        for i in range(self.TREESIZE):
            self.tree.insert("", 0, text=str(i), values=(str(i), str(i), "3", "3", "3", "3", "3", "3", "3", "3", "3", "3"))
        frame.pack()
        self.tree.pack()
        # 添加一个button
        self.btn = tk.Button(self, text='test')
        self.btn.pack(padx=200, pady=30)
        self.btn.config(command=self.test_fun)
 
    def test_fun(self):
        test = {}
        for i in range(30):
            test[i] = (i + 38)
        for item in test:
            print(item)
 
    def get_data(self):
        tm_start = time.time()
        html = self.__wh.getPage()
        soup = self.__hb.getSoup(html)
        inlist = self.__hb.getIndex(soup)
 
        if inlist is not None:
            self.index_list = inlist
        nudict = self.__hb.getNum(soup)
        if nudict is not None:
            self.num_dict = nudict
        tm_end = time.time()
        self.time_cost = tm_end - tm_start
        self.__wh.reflash_web()
 
    def data_loop(self):
        i = 0
        while(self.b_continueget):
            i += 1
            print('loop count:%d' % i)
            self.get_data()
            print(self.index_list)
            print(self.num_dict)
            self.update_tree()
            time.sleep(self.QUEDELAY)
 
    def update_tree(self):
        if self.index_list is not None:
            self.edit_item()
        x = self.tree.get_children()
        # for item in x:
        #     self.tree.delete(item)
        # for i in range(self.TREESIZE):
        #     self.tree.insert("", 0, text=str(i), values=(self.index_list[i], self.num_dict[i], "3", "3", "3", "3", "3", "3", "3", "3", "3", "3"))
 
    def edit_item(self):
        x = self.tree.get_children()
        i = 0
        for item in x:
            if i < len(self.index_list):
                self.tree.item(item, values=(self.index_list[i], self.num_dict[i], "0", "0", "0", "0", "0", "0", "0", "0", "0", "0"))
                i += 1
 
 
if __name__ == '__main__':
    # chrome driver路径
    path = 'C:/Program Files (x86)/Google/Chrome/Application/chromedriver.exe'
    # 抓取的网页url
    url = "https://ds.dsproxy.net/ds_jxf01/#/game/1-3-5"
    app = MainWindows(path, url)
    app.mainloop()
```