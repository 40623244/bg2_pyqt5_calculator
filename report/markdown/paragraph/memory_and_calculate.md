記憶體按鍵處理與直接運算(運算方式)
===

記憶體按鍵處理與直接運算(運算方式) 的概要

1.記憶體按鍵的用法，使我們在做計算時，將我們計算的數值儲存在記憶體中，方便我們更快速運算出所需要的數值。

2.直接運算用法，利用數學上特殊計算法，來運算，例如:開根號、平方、倒數。

3.計算方式，將我們知道的數學運算邏輯，告訴電腦，讓電腦替我們做運算而符合我們的邏輯運算

記憶體按鍵處理
---

記憶體按鍵處理的內容

1.邏輯概念:

clearMemory() 方法與 "MC" 按鍵對應, 清除記憶體中所存 sumInMemory 設為 0

readMemory() 方法與 "MR" 按鍵對應, 功能為讀取記憶體中的數值, 因此將 sumInMemory 顯示在 display, 作為運算數

setMemory() 方法則與 "MS" 按鍵對應, 功能為設定記憶體中的數值，因此取 display 中的數字, 存入 sumInMemory

addToMemory() 方法與 "M+" 按鍵對應, 功能為加上記憶體中的數值, 因此將 sumInMemory 加上 display 中的數值

因為 setMemory() 與 addToMemory() 方法, 都需要取用 display 上的數值, 因此必須先呼叫 equalClicked(), 以更新 sumSoFar 與 display 上的數值

2.設定相對應的按鍵:

    self.clearMemoryButton.clicked.connect(self.clearMemory) - "MC"鍵
     
     self.readMemoryButton.clicked.connect(self.readMemory) - "MR"鍵
     
     self.setMemoryButton.clicked.connect(self.setMemory) - "MS"鍵
     
     self.addToMemoryButton.clicked.connect(self.addToMemory) - "M+"鍵

3.邏輯運用:

def clearMemory(self):

        self.sumInMemory = 0.0
        self.display.setText(str(self.sumInMemory))
        
def readMemory(self):

        self.display.setText(str(self.sumInMemory))
        self.wait = True
        
def setMemory(self):
        self.equalClicked()
        
        self.sumInMemory = float(self.display.text())
        
def addToMemory(self):

        self.equalClicked()
        self.sumInMemory += float(self.display.text())
直接運算
---

直接運算 的內容

1.邏輯概念:

Sqrt, x^2 與 1/x 等按鍵的處理方法為 unaryOperatorClicked(), 與數字按鍵的點按回應相同, 透過 sender().text() 取得按鍵上的 text 字串

unaryOperatorClicked() 方法隨後根據 text 判定運算子後, 利用 display 上的運算數進行運算後, 再將結果顯示在 display 顯示幕上

若進行運算 Sqrt 求數值的平方根時, 顯示幕中為負值, 或 1/x 運算時, x 為 0, 都視為無法處理的情況, 所以需要abortOperation() 處理

abortOperation() 方法則重置所有起始變數, 並在 display 中顯示 "####"
直接運算子處理結束前, 運算結果會顯示在 display 中, 而且運算至此告一段落, 計算機狀態應該要回復到等待新運算數的階段, 因此 waitingForOperand 要重置為 True

2.設定相對應的按鍵:

    unaryOperator = [self.squareRootButton,  self.powerButton,  self.reciprocalButton]
    for i in unaryOperator:
            i.clicked.connect(self.unaryOperatorClicked)

    *squareRootButton - "Sqrt鍵(開根號)"
    *powerButton - "x^2鍵(平方)"
    *reciprocalButton - "1/x鍵(倒數)"
    
3.邏輯運用:

def unaryOperatorClicked(self):

        button = self.sender()
        clickedOperator = button.text()
        operand = float(self.display.text())
        if clickedOperator == "Sqrt":
            if operand < 0.0:
                self.sbortOperand()
                return
            
            result = math.sqrt(operand)
        elif  clickedOperator == "X^2":
            result = math.pow(operand, 2.0)
        elif clickedOperator == "1/x":
            if operand == 0.0:
                self.sbortOperand()
                return
            
            result = 1.0 / operand
            
        self.display.setText(str(result))
        self.wait = True

*中斷運算用:

def abortOperation(self):

        self.clearAll()
        self.display.setText("####")
運算方式
---

運算方式 的內容

1.邏輯概念:

calculate() 方法中的運算, 以 rightOperand 為右運算數
執行加或減運算時, 左運算數為 sumSoFar
執行乘或除運算時, 左運算數為 factorSoFar
若運算過程出現除以 0 時, 將會回傳 False

2.邏輯運用:

def calculate(self, rightOperand, pendingOperator):

        if pendingOperator == "+":
            self.sumSoFar += rightOperand
        elif pendingOperator == "-":
            self.sumSoFar -= rightOperand
        elif pendingOperator == "*":
            self.factorSoFar *= rightOperand
        elif pendingOperator == "/":
            if rightOperand == 0.0:
                return False
            self.factorSoFar /= rightOperand
        return True
心得
---

內容:

這個計算機程式，是一組六個人共同製作，這考驗著六個人的默契，

