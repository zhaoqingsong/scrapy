# branchs
# -*- coding: UTF-8 -*-
import requests
import re
import csv
from bs4 import BeautifulSoup

def get_books():
	books = []
	url = 'https://book.douban.com/tag/小说'
	html = requests.get(url).text
	soup = BeautifulSoup(html,'lxml')
	list_soup = soup.find('ul', {'class': 'subject-list'})	
	for book_info in list_soup.find_all('li'):
		books_author = book_info.find('div',{'class':"pub"})
		book_name = book_info.find(has_class_but_no_id)
		book_url = book_info.find('a',"nbg")
		book_rate = book_info.find('span',{'class':"rating_nums"})
		book_pjs = book_info.find('span',{'class',"pl"})
		book = (book_name.get_text(),books_author.get_text(),book_url.get('href'),book_rate.get_text(),book_rate.get_text())
		books.append(book)
	return books

def book_write(books):
	rows = books
	headers = ["书名","作者","评分","评价人数","豆瓣网URL"]
	with open('book_lists.csv','w',encoding='utf8') as f:
		writer = csv.writer(f)
		writer.writerow(headers)
		writer.writerows(rows)	

if __name__=='__main__':
	books = get_books()
	book_write(books)

