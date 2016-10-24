#!/usr/bin/env python

import socket as skt
import os
import sys
from urlparse import urlparse
import threading
import time
# import srget

if len(sys.argv) ==6 and sys.argv[3] =="-c":

    link = sys.argv[-1]
    FILENAME = sys.argv[2]
    parseStr = urlparse(link)
    URL = parseStr.hostname
    PATH = parseStr.path
    PORT = parseStr.port

    lstOfThread = [x for x in range(int(sys.argv[4]))]
    lstOfFilename = ["filename"+str(x) for x in range(int(sys.argv[4]))]

    tedlst = []


    clientSocket = skt.socket(skt.AF_INET, skt.SOCK_STREAM)
    """basic funtion"""

    def makeNormalGet(url, path):
        return ("GET {n} HTTP/1.1\r\n"+ "Host: {s}\r\n"+"Connection: close\r\n\r\n").format(n=path,s=url)
    def makeRangeGet(url, path):
        pass
    def extractContentLength(header):
        contentLength = ""
        for x in header[header.find("Content-Length")+16:]:
            if x =="\r":
                contentExist = True
                break
            contentLength += x

        return contentLength

    def extractETAG(header):
        etag = ""
        if "\r\n\r\n" in header:
            for x in header[header.find("ETag")+5:]:
                if x =="\r" :
                    break
                etag +=x
        return etag

    def extractLastModified(header):
        lastModified = ""
        if "\r\n\r\n" in header:
            for x in header[header.find("Last-Modified")+14:]:
                if x=="\r":
                    break
                lastModified += x
        return lastModified

    """bassic function"""


    def checkInputForThread():
        if len(sys.argv) == 6:
            return True
        return False

    def checkFile(filename):

        return os.path.exists(filename)



    def manageThread(filename, contentLength, threadNum, url, path, port, lstOfThread):
        start = time.time()

        content_range = contentLength-1
        chunk = content_range//threadNum
        smallChunk = chunk//threadNum




        bigStart = [0] + [x*chunk+1 for x in range(1,threadNum) if x!=0]
        bigStop = [chunk] + [x+chunk-1 for x in bigStart[1:-1]] + [content_range]
        print bigStart
        print bigStop

        lst = [filename+str(x) for x in range(threadNum)]

        try:
            for f,a,b,c in zip(lst, bigStart, bigStop, lstOfThread):
                ted = threading.Thread(target=downloadByPart, args=(f, a, b,url, path, port, c))
                tedlst.append(ted)
                ted.start()
                for ted in tedlst:
                    ted.join


        except KeyboardInterrupt:
            try:
                sys.exit(0)
            except SystemExit:
                os._exit(0)



    def downloadByPart(filename, bigStart, bigStop, url, path, port, ThreadStatus ):
        """download pass by length though request"""




        clientSocket = skt.socket(skt.AF_INET, skt.SOCK_STREAM)
        clientSocket.connect((url, port))
        content = ""
        head = ""

        request  = ("GET {n} HTTP/1.1\r\n"+ "Host: {s}\r\n"+"Connection: close\r\n"+"Range: "+ "bytes=" + str(bigStart)+"-"+str(bigStop)+"\r\n\r\n").format(n=path,s=url)


        clientSocket.send(request)
        con = 0
        try:
            while True:

                data = clientSocket.recv(1)
                head += data

                if "\r\n\r\n" in head:
                    with open(filename, "a+") as f:
                        data = clientSocket.recv(1024)

                        f.write(data)

                        if not data:
                            lstOfThread[ThreadStatus] = True
                            print  lstOfThread
                            print filename, lstOfThread[ThreadStatus], ThreadStatus
                            if lstOfThread == [True for x in range(int(sys.argv[4]))]:
                                print "ohls haha"
                                mergeFile()

                            break

            clientSocket.close()

        except KeyboardInterrupt:
            print "o"
            try:
                sys.exit(0)
            except SystemExit:
                os._exit(0)

        finally :
            clientSocket.close()


    def mergeFile():
        directory = os.getcwd()
        with open(FILENAME, 'a+') as f:
            for x in lstOfFilename:
                with open(x) as k:
                    for line in k:
                        f.write(line)
                os.remove(directory + "/" + x)





    def makeHeader(url, port, path):
        """ makeHeader and extract e tag and contentLength and accept-range"""
        clientSocket.connect((url, port))
        clientSocket.send(makeNormalGet(url, path))
        header = ""
        while "\r\n\r\n" not in header:
            data = clientSocket.recv(1)
            header += data
            if "\r\n\r\n" in header:

                print "close"
                return extractContentLength(header), extractETAG(header), extractLastModified(header)
        clientSocket.close()




    def downloadSimple():
        """ download without thread """

        pass

    def main(url, path, filename, port):
        if checkFile(filename):
            contentLength, etag, Lastmofify = makeHeader(url, port, path)

            if checkInputForThread():
                manageThread()
                downloadByPart()

            else:
                resumeDownload()

        else:
            contentLength, etag, lastModified = makeHeader(url, port, path)


            if checkInputForThread():
                print "this way"
                manageThread(filename, int(contentLength), int(sys.argv[4]), url, path, port,lstOfThread)

            else:
                downloadSimple()

    main(URL, PATH, FILENAME, PORT)
elif sys.argv[3] == '-c':

    link = sys.argv[-1]
    FILENAME = sys.argv[2]
    parseStr = urlparse(link)
    URL = parseStr.hostname
    PATH = parseStr.path
    PORT = parseStr.port

    lstOfThread = [x for x in range(5)]
    lstOfFilename = ["filename"+str(x) for x in range(5)]

    tedlst = []


    clientSocket = skt.socket(skt.AF_INET, skt.SOCK_STREAM)
    """basic funtion"""

    def makeNormalGet(url, path):
        return ("GET {n} HTTP/1.1\r\n"+ "Host: {s}\r\n"+"Connection: close\r\n\r\n").format(n=path,s=url)
    def makeRangeGet(url, path):
        pass
    def extractContentLength(header):
        contentLength = ""
        for x in header[header.find("Content-Length")+16:]:
            if x =="\r":
                contentExist = True
                break
            contentLength += x

        return contentLength

    def extractETAG(header):
        etag = ""
        if "\r\n\r\n" in header:
            for x in header[header.find("ETag")+5:]:
                if x =="\r" :
                    break
                etag +=x
        return etag

    def extractLastModified(header):
        lastModified = ""
        if "\r\n\r\n" in header:
            for x in header[header.find("Last-Modified")+14:]:
                if x=="\r":
                    break
                lastModified += x
        return lastModified

    """bassic function"""


    def checkInputForThread():
        if len(sys.argv) == 5:
            return True
        return False

    def checkFile(filename):

        return os.path.exists(filename)



    def manageThread(filename, contentLength, threadNum, url, path, port, lstOfThread):
        start = time.time()

        content_range = contentLength-1
        chunk = content_range//threadNum
        smallChunk = chunk//threadNum




        bigStart = [0] + [x*chunk+1 for x in range(1,threadNum) if x!=0]
        bigStop = [chunk] + [x+chunk-1 for x in bigStart[1:-1]] + [content_range]
        print bigStart
        print bigStop

        lst = [filename+str(x) for x in range(threadNum)]

        try:
            for f,a,b,c in zip(lst, bigStart, bigStop, lstOfThread):
                ted = threading.Thread(target=downloadByPart, args=(f, a, b,url, path, port, c))
                tedlst.append(ted)
                ted.start()
                for ted in tedlst:
                    ted.join


        except KeyboardInterrupt:
            try:
                sys.exit(0)
            except SystemExit:
                os._exit(0)



    def downloadByPart(filename, bigStart, bigStop, url, path, port, ThreadStatus ):
        """download pass by length though request"""




        clientSocket = skt.socket(skt.AF_INET, skt.SOCK_STREAM)
        clientSocket.connect((url, port))
        content = ""
        head = ""

        request  = ("GET {n} HTTP/1.1\r\n"+ "Host: {s}\r\n"+"Connection: close\r\n"+"Range: "+ "bytes=" + str(bigStart)+"-"+str(bigStop)+"\r\n\r\n").format(n=path,s=url)


        clientSocket.send(request)
        con = 0
        try:
            while True:

                data = clientSocket.recv(1)
                head += data

                if "\r\n\r\n" in head:
                    with open(filename, "a+") as f:
                        data = clientSocket.recv(1024)

                        f.write(data)

                        if not data:
                            lstOfThread[ThreadStatus] = True
                            print  lstOfThread
                            print filename, lstOfThread[ThreadStatus], ThreadStatus
                            if lstOfThread == [True for x in range(5)]:
                                print "ohls haha"
                                mergeFile()

                            break

            clientSocket.close()

        except KeyboardInterrupt:
            print "o"
            try:
                sys.exit(0)
            except SystemExit:
                os._exit(0)

        finally :
            clientSocket.close()


    def mergeFile():
        directory = os.getcwd()
        with open(FILENAME, 'a+') as f:
            for x in lstOfFilename:
                with open(x) as k:
                    for line in k:
                        f.write(line)
                os.remove(directory + "/" + x)





    def makeHeader(url, port, path):
        """ makeHeader and extract e tag and contentLength and accept-range"""
        clientSocket.connect((url, port))
        clientSocket.send(makeNormalGet(url, path))
        header = ""
        while "\r\n\r\n" not in header:
            data = clientSocket.recv(1)
            header += data
            if "\r\n\r\n" in header:

                print "close"
                return extractContentLength(header), extractETAG(header), extractLastModified(header)
        clientSocket.close()




    def downloadSimple():
        """ download without thread """

        pass

    def main(url, path, filename, port):
        if checkFile(filename):
            contentLength, etag, Lastmofify = makeHeader(url, port, path)

            if checkInputForThread():
                manageThread()
                downloadByPart()

            else:
                resumeDownload()

        else:
            contentLength, etag, lastModified = makeHeader(url, port, path)


            if checkInputForThread():
                print "this way"
                manageThread(filename, int(contentLength), 5, url, path, port,lstOfThread)

            else:
                downloadSimple()

    main(URL, PATH, FILENAME, PORT)

else:
    class Downloader(object):
        def __init__(self, url, path, filename, port):
            # self.filename =filename
            self.NL = "\r\n"
            self.url = url
            self.path = path
            self.filename = filename
            self.port = port
            self.header = ""
            self.headerLength = 0
            self.content = ""
            self.contentLength = ""
            self.contentExist = False
            self.clientSocket = skt.socket(skt.AF_INET, skt.SOCK_STREAM)
            self.checkResume = False
            self.headByte = 0
            self.tailByte = 0
            self.etagStatus = False
            self.modifiedStatus = False
            self.etag = ""
            self.oldEtag =""
            self.oldLastModified = ""
            self.lastModified = ""
            self.pair = ""


            self.main()

        def urlVerification(self):
            if self.url[:7] != "http://":
                return "http://" + self.url
            return self.url

        def makeGet(self):
            return ("GET {n} HTTP/1.1\r\n"+ "Host: {s}\r\n"+"Connection: close\r\n\r\n").format(n=self.path,s=self.url)

        def makeResumeGet(self):
            string = ("GET {n} HTTP/1.1\r\n"+ "Host: {s}\r\n"+"Connection: close\r\n"+"Range: bytes={b}-\r\n\r\n").format(n=self.path,s=self.url,b=self.headByte)
            return string


        def Openconnection(self):
            self.clientSocket.connect((self.url, self.port))
            self.clientSocket.send(self.makeGet())

        def openConnectoinResume(self):


            self.clientSocket.connect((self.url, self.port))
            self.clientSocket.send(self.makeResumeGet())

        def closeConnection(self):
            self.clientSocket.close()
            self.content = ""

        def makeHeaderResume(self):
            while "\r\n\r\n" not in self.header:
                data = self.clientSocket.recv(1)
                self.header += data
                if "\r\n\r\n" in self.header:
                    self.extractHeader()
                    self.extractETAG()
                    self.extractLastModified()



        def makeHeader(self):

            self.Openconnection()
            while "\r\n\r\n" not in self.header:
                data = self.clientSocket.recv(1)
                self.header += data
                if "\r\n\r\n" in self.header:
                    self.extractHeader()
                    self.extractETAG()
                    self.extractLastModified()

            # self.closeConnection()



        def extractHeader(self):
            for x in self.header[self.header.find("Content-Length")+16:]:
                if x =="\r":
                    self.contentExist = True
                    break
                self.contentLength += x
            self.headerLength = len(self.header)

        def extractETAG(self):
            if "\r\n\r\n" in self.header:
                for x in self.header[self.header.find("ETag")+5:]:
                    if x =="\r" :
                        break
                    self.etag +=x
            if self.etag != "":
                self.etagStatus = True

        def extractLastModified(self):
            if "\r\n\r\n" in self.header:
                for x in self.header[self.header.find("Last-Modified")+14:]:
                    if x=="\r":
                        break
                    self.lastModified += x

        def downloadWithContent(self):
            string = self.filename
            stringg = string + self.NL +self.etag + self.NL + self.lastModified
            count = 0
            self.filename = open(self.filename, "a+")
            self.pair = open(string + self.NL +self.etag + self.NL + self.lastModified, "wb" )
            try:
                while len(self.content) < int(self.contentLength) :
                    self.content = self.clientSocket.recv(1024)
                    count+= len(self.content)
                    if len(self.content) == 0:
                        curlDir = os.getcwd()

                        os.remove(curlDir+"/"+stringg)
                    if len(self.content) == 0 :
                        break
                    self.filename.write(self.content)

            except KeyboardInterrupt:
                self.pair.write(str(count))



            self.closeConnection()

        def downloadWithoutContent(self):


            while True:
                self.content = self.clientSocket.recv(1024)

                if not len(self.content) == 0:
                    break

                self.file.write(self.content)
            self.closeConnection()

        def handleError(self):
            pass


        def checkResumeCondition(self):

            if os.path.isfile(self.filename) :

                self.headByte = os.path.getsize(self.filename)
                self.checkResume = True
        def checkOldval(self):
            oldFile = self.filename+self.NL+self.etag+self.NL+self.lastModified

            if os.path.exists(oldFile):
                self.checkResume = True
                lst =[]
                s = ''
                string = oldFile
                lst = string.split(self.NL)
                self.oldEtag = lst[-2]
                self.oldLastModified =lst[-1]
                with open(oldFile,"r") as f:
                    self.headByte = f.readlines()

            else:
                print "Successfully Done"



        def resumeDownload(self):
            self.openConnectoinResume()
            self.makeHeaderResume()
            self.checkOldval()
            f = open(self.filename, "a+")
            if self.oldEtag == self.etag or self.lastModified ==self.oldLastModified :
                try:
                    while True :
                        stringg = (self.filename) + self.NL +self.etag + self.NL + self.lastModified
                        self.content = self.clientSocket.recv(1024)
                        f.write(self.content)
                        if len(self.content) == 0:
                            curlDir = os.getcwd()
                            os.remove(curlDir+"/"+stringg)

                        if not self.content:
                            break



                    self.closeConnection()
                except KeyboardInterrupt:
                    self.pair.write(str(count))


        def checkPort(self):
            if self.port == None:
                self.port = 80

        def main(self):


            self.checkPort()
            self.urlVerification()
            self.checkResumeCondition()
            if self.checkResume == True:


                self.resumeDownload()



            if self.checkResume == False:
                self.makeHeader()
                if self.contentExist:
                    self.downloadWithContent()

                if self.contentExist == False:
                    self.downloadWithoutContent()






    link = sys.argv[-1]
    FILENAME = sys.argv[-2]
    parseStr = urlparse(link)
    URL = parseStr.hostname
    PATH = parseStr.path
    PORT = parseStr.port

    a = Downloader(URL, PATH, FILENAME, PORT)
