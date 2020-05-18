import json
import os
import os.path
import shutil
import re

numerals = set('1234567890-')
illegalCharacters = set('#^&*()<>?/\:;"')

DefaultFolder = os.getcwd() + "\\"

# import filesystem
with open("WorkshopFileInfos.json") as masterFile:
    masterList = json.load(masterFile)

# print(masterList)


# create workshop name translations aka keyRing
keyRing = []
for x in range(len(masterList)):
    # Get mod name
    modInfo = []
    modData = masterList[x]
    nameData = modData["Name"]
    # get mod id number
    idData = modData["Directory"]
    idData = idData.split("\\")
    idData = (idData[len(idData)-1]).split(".")[0]

    # add data to new keyRing object
    modInfo.append(idData)
    modInfo.append(nameData)

    # add keyRing object to keyRing
    keyRing.append(modInfo)

# print(keyRing)

# get user input

# userSelection = input("Input search String\n")
"""
for x in range(len(keyRing)):
    if ((keyRing[x])[0]) == userSelection:
        print("FOUND\n", (keyRing[x])[1])
"""

# Menu
exitFlag = True
while exitFlag:
    directoryList = []
    fileList = []

    # set directory
    thisDirectory = os.listdir(".")

 # get list of files
    for fileName in thisDirectory:
        if os.path.isfile(os.path.join(os.path.abspath("."), fileName)):
            fileList.append(fileName)

    # clean filenames
    x = 0
    for x in range(len(fileList)):
        fileList[x] = fileList[x].split(".")[0]

    # remove duplicates
    listLength = len(fileList)
    x = 0

    # replace with with int range dummy
    while x < listLength:
        if x+1 < len(fileList):
            if fileList[x+1] == fileList[x]:
                fileList.remove(fileList[x+1])
                listLength = listLength-1
            else:
                x = x + 1
        else:
            x = x + 1

    # translate filenames to mod names
    for x in range(len(fileList)):
        for y in range(len(keyRing)):
            if fileList[x] == (keyRing[y])[0]:
                fileList[x] = (keyRing[y])
                break

    # Clean all files that were not part of the keyRing
    fileList = [x for x in fileList if type(x) != str]

    # print the file list
    print("\nFiles: ")
    for x in range(len(fileList)):
        print(x, ": ", (fileList[x])[1])

    print("\nCurrent Folder: ", os.getcwd())
    # get list of directories

    for dirName in thisDirectory:
        if os.path.isdir(os.path.join(os.path.abspath("."), dirName)):
            directoryList.append(dirName)

    # print directories
    print("\nSubfolders:")
    for x in range(len(directoryList)):
        print(x, ": ", directoryList[x])

    # set directory and file list sizes
    directorySize = len(directoryList)
    fileListSize = len(fileList)

    # Menu prompt
    loopFlag = True
    while loopFlag:
        userInput = input("\nInput option\n1: Change directory\n2: Move Files\n3: Create Directory\n0: Exit\n")

        # change user directory
        if userInput == '1':
            userInput = input("Choose Directory, enter -1 to go up one file\nenter -2 to return to home\n")
            if userInput == "-1":
                os.chdir("..")
            elif userInput == "-2":
                os.chdir(DefaultFolder)
            elif int(userInput) <= directorySize:
                systemDestination = "./"
                systemDestination = systemDestination + directoryList[int(userInput)]
                os.chdir(systemDestination)
            loopFlag = False
        # execute user option

        # User options
        # move files
        elif userInput == '2':
            userInput = input("Choose files you would like to move\nEnter file numbers separated by commas"
                              "\nTo select a range enter beginning of range and end of range separated by -\nexample "
                              "\"2-11\"\n")
            moveList = str.split(userInput, ',')

            # clean and prep movelist
            for x in moveList:
                if set(x.strip()).issubset(numerals) and set(x.strip()) != set():
                    # clean to valid input
                    if (x.strip()).isnumeric():
                        None
                    else:
                        values = x.split('-')
                        start = values[0]
                        end = values[1]
                        """multiSelection = []
                        multiSelection.append(start)
                        multiSelection.append(end)"""
                        if int(start) < int(end):
                            multiSelection = [start, end]
                        else:
                            multiSelection = [end, start]
                        list.remove(moveList, x)
                        list.insert(moveList, 0, multiSelection)
                elif set(x) != set():
                    # junk item
                    list.remove(moveList, x)
            # Clean movelist
            x = len(moveList)
            moveList = [x for x in moveList if x != []]

            # Choose Destination
            destinationLoopFlag = True
            while destinationLoopFlag:
                userInput = input("Choose Destination Folder\nEnter -1 for Parent Folder\nEnter -2"
                " to start from root workshop folder\n")
                if int(userInput.strip()) >= -2 and int(userInput.strip()) < len(directoryList):
                    # move files
                    moveListSize = len(moveList)
                    moveX = 0

                    # set file destination
                    if int(userInput.strip()) == -1:
                        systemDestinationPrime = "..\\"
                    elif int(userInput.strip()) == -2:
                        systemDestinationPrime = DefaultFolder
                    else:
                        systemDestinationPrime = ".\\"
                        systemDestinationPrime = systemDestinationPrime + directoryList[int(userInput.strip())] + "\\"

                    #sub folder selection
                    # get subfolders

                    subFolderFlag = True

                    while subFolderFlag:
                        subFolders = [name for name in os.listdir(systemDestinationPrime) if
                                      os.path.isdir(os.path.join(systemDestinationPrime, name))]
                        dirNum = 0
                        print("subFolders:\n")
                        for name in subFolders:
                            print(dirNum, ":", name)
                            dirNum += 1
                        userInput = input("Would you like to select a subfolder?\n0:No\n1:Yes\n")
                        if int(userInput.strip()) == 1:

                            if len(subFolders) != 0:
                                userInput = input("Choose Subfolder\nSelect -1 to cancel\n")
                                if int(userInput.strip()) == int(-1):
                                    subFolderFlag = False
                                else:
                                    systemDestinationPrime = systemDestinationPrime + subFolders[int(userInput.strip())] +"\\"
                            else:
                                print("No subfolders are present")
                                subFolderFlag = False
                        else:
                            subFolderFlag = False


                    # expand moveList
                    expandedMoveList = []
                    for x in moveList:
                        if isinstance(x, list):
                            start = int(x[0])
                            end = int(x[1])
                            while start <= end:
                                expandedMoveList += [str(start)]
                                start += 1
                        else:
                            expandedMoveList += [str(x.strip())]
                    moveList = expandedMoveList
                    moveListSize = len(moveList)
                    while moveX < moveListSize:
                            srcFile = ".\\"
                            fileListLocation = int(moveList[moveX].strip())
                            if fileListLocation < len(fileList):
                                fileName = (fileList[fileListLocation])[0]
                                srcFile = srcFile + fileName
                                systemDestination = systemDestinationPrime
                                systemDestination += fileName
                                if os.path.exists(srcFile + ".png"):
                                    shutil.move(srcFile + ".png", systemDestination + ".png")
                                    print(srcFile, ".png moved to ", systemDestination, ".png")
                                if os.path.exists(srcFile + ".cjc"):
                                    shutil.move(srcFile + ".cjc", systemDestination + ".cjc")
                                    print(srcFile, ".cjc moved to ", systemDestination, ".cjc")
                                if os.path.exists(srcFile + ".json"):
                                    shutil.move(srcFile + ".json", systemDestination + ".json")
                                    print(srcFile, ".json moved to ", systemDestination, ".json")
                            moveX += 1
                    destinationLoopFlag = False
                else:
                    print("Invalid Folder")
            loopFlag = False

        # create directory
        elif userInput == '3':
            validFlag = True
            while validFlag:
                userInput = input("Enter path name\nEnter 0 to exit\n")

                testSet = illegalCharacters.intersection(set(userInput), illegalCharacters)

                if set(userInput).intersection(set(userInput), illegalCharacters):
                    print("Illegal Input")
                else:
                    if userInput == "0":
                        validFlag = False
                    else:
                        print("Folder Created")
                        folderPath = "./"
                        folderPath = folderPath + userInput
                        os.mkdir(folderPath)
                        validFlag = False
                        loopFlag = False

        # delete directory
        elif userInput == '0':
            loopFlag = False
            exitFlag = False

# for name in os.listdir("."):
    # print(name)

# while loopFlag:
    # print("enter mod names seperated by commas you would like to move")

# print("done")
