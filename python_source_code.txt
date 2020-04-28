from random import randint

def randomThrow():
    firstDice = randint(1,6)
    secondDice = randint(1,6)
    return firstDice+secondDice

allFields = [0]*40
current = 0
i = 0

currentChanceCard = 1
currentOTcard = 1

chanceCardFile = open("chancecards.txt")
chanceLines = chanceCardFile.readlines()

otCardFile = open("otcards.txt")
otLines = otCardFile.readlines()

def incrementCard(ourCard):
    if ourCard !=16:
        ourCard += 1
    elif ourCard == 16:
        ourCard = 1

while i<10000:
    thisThrow = randomThrow()
    if current + thisThrow == 30:
        allFields[30] += 1
        current = 10
        allFields[current] += 1
    elif current + thisThrow == 7 or current + thisThrow == 22 or current + thisThrow == 36 or (current - 40 + thisThrow == 7):
        if current + thisThrow == 7 or current + thisThrow == 22 or current + thisThrow == 36:
            allFields[current + thisThrow] += 1
        elif current - 40 + thisThrow == 7:
            allFields[7] += 1
        chanceActivity = chanceLines[currentChanceCard].split('\t')
        if chanceActivity[1] == 'gotolondon':
            current = 24
            allFields[current] += 1
            incrementCard(currentChanceCard)
        elif chanceActivity[1] == 'gotostart':
            current = 0
            allFields[current] += 1
            incrementCard(currentChanceCard)
        elif chanceActivity[1] == 'gotojail':
            current = 10
            allFields[current] += 1
            incrementCard(currentChanceCard)
        elif chanceActivity[1] == 'gotoclosesttransport':
            if current + thisThrow == 7:
                current = 15
            elif current + thisThrow == 22:
                current = 25
            elif current + thisThrow == 36:
                current = 5
            allFields[current] += 1
            incrementCard(currentChanceCard)
        elif chanceActivity[1] == 'gothreeb':
            current = current + thisThrow - 3
            allFields[current] += 1
            incrementCard(currentChanceCard)
        elif chanceActivity[1] == 'gotoistanbul':
            current = 11
            allFields[current] += 1
            incrementCard(currentChanceCard)
        elif chanceActivity[1] == 'gotomontreal':
            current = 39
            allFields[current] += 1
            incrementCard(currentChanceCard)
        elif chanceActivity[1] == 'gotojelez':
            current = 5
            allFields[current] += 1
            incrementCard(currentChanceCard)
        elif chanceActivity[1] == 'gotoelectricity':
            if current + thisThrow == 7 or current + thisThrow == 36:
                current = 12
            elif current + thisThrow == 22:
                current = 28
            allFields[current] += 1
            incrementCard(currentChanceCard)
        else:
            current = current + thisThrow
            incrementCard(currentChanceCard)
    elif current + thisThrow == 2 or current + thisThrow == 17 or current + thisThrow == 33 or (current - 40 + thisThrow == 2):
        if current + thisThrow == 2 or current + thisThrow == 17 or current + thisThrow == 33:
            allFields[current + thisThrow] += 1
        elif current - 40 + thisThrow == 2:
            allFields[2] += 1
        otActivity = otLines[currentOTcard].split('\t')
        if otActivity[1] == 'gotojail':
            current = 10
            allFields[current] += 1
            incrementCard(currentOTcard)
        elif otActivity[1] == 'gotostart':
            current = 0
            allFields[current] += 1
            incrementCard(currentOTcard) 
        else:
            current = current + thisThrow
            incrementCard(currentOTcard)
    elif current + thisThrow > 39:
        current = current - 40 + thisThrow
        allFields[current] += 1
    else:
        current = current + thisThrow
        allFields[current] += 1
    i += 1

resultFile = open("results.txt", "w+")

for each in range(0,40):
    resultFile.write(str(each) + '\t' + str(allFields[each]) + '\n')
    print str(each) + '\t' + str(allFields[each]) + '\n'


    