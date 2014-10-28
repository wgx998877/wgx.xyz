title : 使用python进行数据处理
tags: [python,data,数据处理]
---
# 使用python工具进行数据处理
## 简介
使用python工具将txt中观测站点对应遥感数据行列号提取，并输出每个站点所有对应数据行列号文件

## 代码 code
	#coding:utf8
	import os
	class sta():
		def __init__(self,name=''):
			self.point_list = []
			self.name = name
	s_list = []
	path = '/home/wgx/code/hangliehao'
	for i in os.listdir(path):
		if '.txt' not in i:
			continue
		model = i.split('.')[0]
		f = open(i,'r')
		for j in f:
			#print j
			try:
				tmp = (','.join(j.replace('\r\n','').split())).split(',')#去除空行，分割字符串
				is_in = False
				s = sta(tmp[0])
				for k in s_list:
					if k.name == tmp[0]:
						s = k
						is_in = True
						break
				s.point_list.append([model,tmp[1],tmp[2]])
				if is_in == False:
					s_list.append(s)
			except:
				print 'error %s:%s' % i,j
				pass
		f.close()
	print len(s_list)
	for i in s_list:
		f = open(path+'/out/'+i.name+'.txt','w')
		f.write("%16s  %6s  %6s  \n" % ("model","simple","line"))
		for j in i.point_list:
		#	print j
			mystr = ("%16s  %6s  %6s  \n" % (j[0],str(j[1]),str(j[2])))
			f.write(mystr)
		f.close()
	print 'done!'
