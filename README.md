# Python_tools
small tool create by python
===========================
1.如果对文件夹进行备份,难免就会有版本出现新旧不匹配的问题,这个工具就是为了保证两个文件夹文件一致.

****
xfgao@163.com	
****
## 目录
* [# 同步文件夹](#同步文件夹)


## 同步文件夹
### code

![Syn_folder.py](https://github.com/45717335/Python_tools/blob/main/Syn_folder.py"Syn_folder.py")
```python
import os
import sys
import time
import shutil
import csv
def tongbu_folder(fdf,fdt):
    if fdf[-1:] != "\\":
        fdf = fdf + "\\"
    if fdt[-1:] != "\\":
        fdt = fdt + "\\"
    if fdf.split("\\")[-2]!=fdt.split("\\")[-2]:
        print("Must have the same root folder name:{0}!={1}".format(fdf.split("\\")[-2],fdt.split("\\")[-2]) )
        return
    file_from,file_to,fd_from,fd_to,fd1,fd2,fd3="","","","","","",""
    if os.path.exists(fdt) == False:
        os.makedir(fdt)
    for root, dirs, files in os.walk(fdf):
        #如果根目录的最后修改时间和目标的不同则,复制文件后修改他的时间变成相同.
        fd_from=root
        fd1=root[len(fdf):len(root)]
        fd_to=os.path.join(fdt,fd1)
        #如果不存在则创建
        if os.path.exists(fd_to)==False:
            os.mkdir(fd_to)
        if  time.ctime(os.path.getmtime(fd_from)) !=time.ctime(os.path.getmtime(fd_to)):
        # 如果from和to的最后修改日期不一样,则:1同步文件,2同步文件夹,3修改文件夹时间
            #1.遍历To,如果不在from中或者时间不对,则删除它
            for root1,dirs1,files1 in os.walk(fd_to):
                for file1 in files1:
                    try:
                        os.remove(os.path.join(root1,file1))
                    except Exception as ex:
                        try:
                            os.system("del "+os.path.join(root1,file1)+" /F")
                        except Exception as ex:
                            print(str(ex))
                for dir1 in dirs1:
                    if os.path.exists(os.path.join(root,dir1))==False:
                        shutil.rmtree(os.path.join(root1,dir1))
                break
            for file in files:
                try:
                    shutil.copy2(os.path.join(fd_from,file),os.path.join(fd_to,file))
                except Exception as ex:
                    print(str(ex))
        os.utime(fd_to,(os.path.getatime(fd_from),os.path.getmtime(fd_from)))
if __name__ == '__main__':
    #如果输入的是CSV则直接从CSV中进行同步
    #如果不是CSV 则判断它是不是一个路径,如果是路径则将其当作FROM 文件夹,并要求 输入TO文件夹, 循环至 输入的不是一个文件夹后,
    #将list显示出来,之后进行同步
    #print code
    f1,t1,li1="","",[]
    print( "*"*50 + "\nFunction is BACK UP FILES,(FROM -> TO)")
    f1=input("please input .csv or from folder:")
    if f1.endswith(".csv"):
        f = csv.reader(open(f1, 'r'))
        li1 = list(f)
    else:
        t1=input("please input to folder:")
        while os.path.exists(f1) and os.path.exists(t1):
            li1.append([f1,t1])
            f1,t1=input("please input From folder:"),input("please input To folder:")
    print(li1)
    if len(li1)>0 and input("[ from,to ]\n Continue to Syn_folder (Y/N)")=="Y" :
        for x1 in li1:
            t1=time.time()
            if len(x1)==1:
                x1=x1[0].split(";")
                tongbu_folder(x1[0],x1[1])
            else:
                tongbu_folder(x1[0],x1[1])
            t2=time.time()
            print("finish syn folder time:{0} \n from: {1} \n to: {2} ".format(t2-t1,x1[0],x1[1]))
    input("input any key to continue.")
  ```
### Download
.7z 密码:PASSWORD
![下载同步文件夹](https://github.com/45717335/Python_Candle/blob/main/test3_v4.py_(output).png "下载同步文件夹")
