# memory_card1
from PyQt5.QtCore import Qt
from PyQt5.QtWidgets import (
        QApplication, QWidget, 
        QHBoxLayout, QVBoxLayout, 
        QGroupBox, QRadioButton,  
        QPushButton, QLabel,QButtonGroup)
from random import shuffle , randint
class Question():
    def __init__(self,question,right_answer,wrong1,wrong2,wrong3):
        self.question= question
        self.right_answer = right_answer
        self.wrong1 = wrong1
        self.wrong2 = wrong2
        self.wrong3 = wrong3
question_list = []
question_list.append(Question('Государственный язык Бразилии' ,'Бразильский','Португальский','Английский','Америнканский'))
question_list.append(Question('Какого цвета нет на флаге Беларуси' ,'Синий','Красный','Зелёный','Белый'))
question_list.append(Question('Национальная хижина якутов' ,'Юрта','Ураса','Иглу','Хата'))
points = 0
queston_count = 0
app = QApplication([])
btn_OK = QPushButton('Ответить')
lb_Question = QLabel('Самый сложный вопрос в мире!')
RadioGroupBox = QGroupBox("Варианты ответов")
rbtn_1 = QRadioButton('Вариант 1')
rbtn_2 = QRadioButton('Вариант 2')
rbtn_3 = QRadioButton('Вариант 3')
rbtn_4 = QRadioButton('Вариант 4')
RadioGroup = QButtonGroup()
RadioGroup.addButton(rbtn_1)
RadioGroup.addButton(rbtn_2)
RadioGroup.addButton(rbtn_3)
RadioGroup.addButton(rbtn_4)
layout_ans1 = QHBoxLayout()   
layout_ans2 = QVBoxLayout()
layout_ans3 = QVBoxLayout()
layout_ans2.addWidget(rbtn_1)
layout_ans2.addWidget(rbtn_2)
layout_ans3.addWidget(rbtn_3) 
layout_ans3.addWidget(rbtn_4) 
layout_ans1.addLayout(layout_ans2)
layout_ans1.addLayout(layout_ans3)
RadioGroupBox.setLayout(layout_ans1)
AnsGroupBox = QGroupBox("Результат теста")
lb_Result = QLabel('прав ты или нет?') 
lb_Correct = QLabel('ответ будет тут!') 
ResultGroupBox = QGroupBox('Результат тестирования')
Ib_Result_Test = QLabel('')
layout_res = QVBoxLayout()
layout_res.addWidget(lb_Result, alignment=(Qt.AlignLeft | Qt.AlignTop))
layout_res.addWidget(lb_Correct, alignment=Qt.AlignHCenter, stretch=2)
AnsGroupBox.setLayout(layout_res)
layout_line1 = QHBoxLayout()
layout_line2 = QHBoxLayout()
layout_line3 = QHBoxLayout()
layout_line1.addWidget(lb_Question, alignment=(Qt.AlignHCenter | Qt.AlignVCenter))
layout_line2.addWidget(RadioGroupBox)   
layout_line2.addWidget(AnsGroupBox)  
AnsGroupBox.hide()
layout_line3.addStretch(1)
layout_line3.addWidget(btn_OK, stretch=2)
layout_line3.addStretch(1)
layout_card = QVBoxLayout()
layout_card.addLayout(layout_line1, stretch=2)
layout_card.addLayout(layout_line2, stretch=8)
layout_card.addStretch(1)
layout_card.addLayout(layout_line3, stretch=1)
layout_card.addStretch(1)
layout_card.setSpacing(5)
def  show_result():
    RadioGroupBox.hide()
    AnsGroupBox.show()
    btn_OK.setText('Следующий вопрос')
def show_question():
    RadioGroupBox.show()
    AnsGroupBox.hide()
    btn_OK.setText('Ответить')
    rbtn_1.setChecked(False)
    rbtn_2.setChecked(False)
    rbtn_3.setChecked(False)
    rbtn_4.setChecked(False)
    RadioGroup.setExclusive(True)
answers = [rbtn_1,rbtn_2,rbtn_3,rbtn_4]
def ask(q: Question):
    shuffle(answers)
    answers[0].setText(q.wrong1)
    answers[1].setText(q.wrong2)
    answers[2].setText(q.right_answer)
    answers[3].setText(q.wrong3)
    Ib_Question.setText(q.question)
    Ib_Correct.setText(q.right_answer)
    show_question()
def show_correct(res):
    Ib_Result.setText(res)
    show_result()
def check_answer():
    global points , queston_count
    if answers[2].isChecked():
        points +=1
        questiun_count+=1
        show_correct('Правильно!У вас ' + str(points) , 'очков')
    else:
        questiun_count +=1
        if answers[0].isChecked() or answers[1].isChecked() or answers[3].isChecked():
            show_correct('Неверно ! У вас ' + str(points) , 'очков')
def next_question():
    if len(question_list) == 0:
        show_test_result()
    window.cur_question = randint(0,len(question_list)-1)
    q = question_list[window.cur_question]
    ask(q)
def show_test_result():
    global points, queston_count
    RadioGroupBox.hide()
    AnsGroupBox.hide()
    Ib_Result_Text.setText('Ваш результат:' + str(points) + 'из' + str(queston_count))
    ResultGroupBox.show()
def click_OK():
    if btn_OK.text() == 'Ответить' :
        check_answer()
    else:
        next_question()
window = QWidget()
window.setLayout(layout_card)
window.setWindowTitle('Memory Card')
btn_OK.clicked.connect(click_OK)
window.show()
app.exec()
