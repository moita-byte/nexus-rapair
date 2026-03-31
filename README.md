import sys
import mysql.connector
from PyQt5 import uic,QtWidgets


def conectar():

  # Funçao reutilizavel de conexao 
  return mysql.connector.connect(
        host = "localhost",
        user="root",
        password="",
        database="bd_escola"
    )

def salvar():

    nome = tela.txt_nome.text()
    idade = tela.txt_idade.text()
    curso = tela.txt_curso.text()


    conexao = conectar()
    cursor = conexao.cursor()

    comando = "INSERT INTO alunos (nome,idade,curso)VALUES (%s,%s,%s)"

    dados = (nome,idade,curso)

    cursor.execute(comando,dados)

    conexao.commit()


    QtWidgets.QMessageBox.information(tela,"Sucessso","Aluno cadastrado")
    
    
def listar():

        conexao = conectar()
        cursor = conexao.cursor()

        cursor.execute("SELECT * FROM alunos")

        dados = cursor.fetchall()

        tela.tabela_aluno.setRowCount(len(dados))


        for i in range(len(dados)):


            for j in range(4):


                    tela.tabela_aluno.setItem(i,j, 
 QtWidgets.QTableWidgetItem(str(dados[i][j]))          )

def excluir():
                
        linha = tela.tabela_aluno.currentRow()

        id_aluno = tela.tabela_aluno.item(linha, 0).text()
    
        conexao = conectar()
        cursor = conexao.cursor()
             
             
        comando = "DELETE FROM alunos WHERE id = %s"  

        cursor.execute(comando, (id_aluno,))

        conexao.commit()

        listar()


app = QtWidgets.QApplication([])

tela = uic.loadUi("sistema_alunos.ui")

tela.btn_salvar.clicked.connect(salvar)
tela.btn_listar.clicked.connect(listar)
tela.btn_excluir.clicked.connect(excluir)

tela.show()

app.exec()
