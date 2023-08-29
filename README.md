# Jogo de dados multiplayer Python

import tkinter as tk
from tkinter import *
from tkinter import ttk
from PIL import Image, ImageTk
from random import randint
import time
import pandas as pd 
 

class Inicio: 
    def __init__(self, mestre):        
        
        self.mestre = mestre
        self.mestre.title('Menu')
        self.mestre.geometry("290x290")
        self.mestre.iconphoto(False, PhotoImage(file=r"C:\Users\sotam\OneDrive\Desktop\Códigos\Cassino\dado.png"))
        self.mestre.resizable(width=False, height=False)
        self.mestre.config(bg='#228b22')
        
        # Construção da imagem de tela de fundo
        
        self.logo = Image.open(r"C:\Users\sotam\OneDrive\Desktop\Códigos\Cassino\logo_.png")  # Coloque o caminho da imagem do dado aqui
        self.logo = self.logo.resize((180, 180), Image.ANTIALIAS)
        self.logo = ImageTk.PhotoImage(self.logo)

        # construção dos widgets
        self.foto = tk.Label(self.mestre, image=self.logo, bg='#228b22')
        self.foto.pack()
        self.botao1 = tk.Button(self.mestre, text="Jogo livre", command= self.abrir_simulador, bg='#cc0000', font=("Helvetica",12,'bold'), width=15)
        self.botao1.pack(pady=2)
        self.botao2 = tk.Button(self.mestre, text="Competitivo", command= self.abrir_2, bg='#cc0000', font=("Helvetica",12,'bold'),width=15)
        self.botao2.pack()
    
    def abrir_simulador (self):
        self.mestre.withdraw()
        self.simulador = tk.Toplevel(self.mestre)
        SimuladorDado(self.simulador)
    
    def abrir_2 (self):
        self.mestre.withdraw()
        self.sorteio = tk.Toplevel(self.mestre)
        Sorteio1(self.sorteio, self.mestre)        


class Sorteio1:
    def __init__(self, chefe, janela_mestre):
        #Construção da Janela
        
        self.chefe = chefe
        self.chefe.title('Sorteio')
        self.chefe.geometry("290x290")
        self.chefe.iconphoto(False, PhotoImage(file=r"C:\Users\sotam\OneDrive\Desktop\Códigos\Cassino\dado.png"))
        self.chefe.resizable(width=False, height=False)
        self.chefe.config(bg='#228b22')

        #Construção da imagem
        self.icone = Image.open(r"C:\Users\sotam\OneDrive\Desktop\Códigos\Cassino\logo_.png")
        self.icone = self.icone.resize((45, 40), Image.ANTIALIAS)
        self.icone = ImageTk.PhotoImage(self.icone)

        # Criar componente ícone de home
        self.button3 = tk.Button(self.chefe, image= self.icone, relief=FLAT, command=self.abrir_menu, bg='#228b22')
        self.button3.pack(anchor='nw', padx=1, pady=1)

        #construção do widget
        self.titulo = tk.Label(self.chefe, text="Nome do Jogador", font=("Helvetica", 20), bg='#228b22')
        self.titulo.pack(pady=20)

        self.entrada = tk.Entry(self.chefe)
        self.entrada.pack(pady=10)
        
        self.button1 = tk.Button(self.chefe, text="Adicionar", command= self.save_player, bg='#cc0000', font=("Helvetica",12,'bold'), width= 13)
        self.button1.pack(pady=2)
        
        self.button2 = tk.Button(self.chefe, text="Sortear", command= self.abrir_sortear, bg='#cc0000', font=("Helvetica",12,'bold'), width=13)
        self.button2.pack()

        #Construção de um dataframe para armazenar os jogadores
        self.lista = {'Jogador':[], 'Resultado':[]}
        self.df = pd.DataFrame(self.lista)
      

        #construção do referenciamento à janela mestre
        self.janela_mestre = janela_mestre

    def abrir_menu(self):
         self.chefe.withdraw()
         root.deiconify()
         

    def save_player(self):
            jogador = self.entrada.get()
            self.df = self.df.append({'Jogador':jogador}, ignore_index=True)
            self.entrada.delete(0, END)
        
    def abrir_sortear (self):
            self.chefe.withdraw() #esconde a janela atual
            self.sorteio_janela = tk.Toplevel(self.chefe) #cria a nova janela
            Sorteio2(self.sorteio_janela, self.df, self.janela_mestre)
                        

class Sorteio2: 
    def __init__(self, dono, df, janela_mestre):
        #construção da janela
        self.dono = dono
        self.dono.title("Lançamento do Dado")
        self.dono.geometry("290x290")
        self.dono.iconphoto(False, PhotoImage(file=r"C:\Users\sotam\OneDrive\Desktop\Códigos\Cassino\dado.png"))
        self.dono.resizable(width=False, height=False)
        self.dono.config(bg='#228b22')
        self.df = df
        self.janela_mestre = janela_mestre

        #Construção da imagem
        self.icone = Image.open(r"C:\Users\sotam\OneDrive\Desktop\Códigos\Cassino\logo_.png")
        self.icone = self.icone.resize((45, 40), Image.ANTIALIAS)
        self.icone = ImageTk.PhotoImage(self.icone)

        # Criar componente ícone de home
        self.button3 = tk.Button(self.dono, image= self.icone, relief=FLAT, command=self.abrir_menu, bg='#228b22')
        self.button3.pack(anchor='nw', padx=1, pady=1)

        #tratamento da imagem
        self.imagem_dado = Image.open(r"C:\Users\sotam\OneDrive\Desktop\Códigos\Cassino\dado.png")  # Coloque o caminho da imagem do dado aqui
        self.imagem_dado = self.imagem_dado.resize((100, 100), Image.ANTIALIAS)
        self.imagem_dado = ImageTk.PhotoImage(self.imagem_dado)

        #Construção dos widgets
        self.label_imagem = tk.Label(self.dono, image=self.imagem_dado, bg='#228b22')
        self.label_imagem.pack(pady=1)

        self.label_resultado = tk.Label(self.dono, text="", font=("Helvetica", 48), bg='#228b22')
        self.label_resultado.pack(pady=8)
        
        self.botao_lancar = tk.Button(self.dono, text="Lançar Dado", command=self.lancar_dado, bg='#cc0000', font=("Helvetica",12,'bold'), width=14)
        self.botao_lancar.pack(pady=2)

        #Construção da lista bode expiatória para contagem dos cliques
        self.contador=[]


    def lancar_dado(self):
        # Loop de resultados
        for _ in range(10):
            resultado = randint(1, 6)
            self.label_resultado["text"] = str(resultado)
            self.dono.update()
            time.sleep(0.3)    
        # imputação dos resultados no banco de dados
        self.df = self.df.fillna(0) 
        posicao = self.df['Resultado'].idxmin()
        self.df.loc[posicao,'Resultado'] = resultado
        # adição de itens na lista
        self.contador.append('a')
        # criação das variáveis condicionais para abertura da janela de resultados
        self.cliques = len(self.contador)
        self.player = len(self.df)
        # condicional de abertura de janela
        if self.cliques == self.player:
            self.show_results()
        
    # pop up de mostrar resultados
    def show_results(self):
        #Colocar o valor das tabelas em ordem
        self.df.sort_values(by=["Resultado"], ascending=False, inplace=True)

        #abrir uma nova janela
        self.resultado = tk.Toplevel(self.dono)
        Sorteados(self.resultado, self.df, self.janela_mestre)
    
    def abrir_menu(self):
         self.dono.withdraw()
         root.deiconify()
         

class Sorteados:
    def __init__(self, owner, df, janela_mestre):
         
         #Janela de resultados e suas configurações
         self.owner = owner
         self.owner.title = ('Resultados')
         self.owner.geometry("220x220")
         self.owner.iconphoto(False, PhotoImage(file=r"C:\Users\sotam\OneDrive\Desktop\Códigos\Cassino\dado.png"))
         self.owner.resizable(width=False, height=False)
         self.owner.config(bg='#228b22')
         self.df = df
         self.janela_mestre = janela_mestre

         # criação das definições de layout do treeview
         def tabela_layout():
              estilo = ttk.Style()
              estilo.theme_use('default')

              # Alterar a cor de fundo do cabeçalho
              estilo.configure('Treeview.Heading', background='red', foreground='black', font=('Gotham',12,'bold'))
              # alterar a cor de fundo das linhas alternadas
              estilo.configure("Treeview", background='#228b22', font=('Gotham',8,'bold'))

         # Widgets e resultado
         self.tabela = ttk.Treeview (self.owner, columns= (1,2), show="headings")
         self.tabela.heading(1, text='Jogador')
         self.tabela.heading(2, text='Resultado')
         self.tabela.column(1, width=15)
         self.tabela.column(2, width=15)

         for index, row in self.df.iterrows():
              self.tabela.insert('', tk.END, values=[row['Jogador'],row['Resultado']])
         # Aplicando as definições de Layout na tabela
         self.tabela.pack(fill="both", expand = True) 

         #chamando a função para aplicar as configurações de layout
         tabela_layout()
         

class SimuladorDado:
    def __init__(self, master):
        self.master = master
        self.master.title("Simulador de Dado")
        self.master.geometry("290x290")
        self.master.iconphoto(False, PhotoImage(file=r"C:\Users\sotam\OneDrive\Desktop\Códigos\Cassino\dado.png"))
        self.master.resizable(width=False, height=False)
        self.master.config(bg='#228b22')

        #Construção da imagem
        self.icone = Image.open(r"C:\Users\sotam\OneDrive\Desktop\Códigos\Cassino\logo_.png")
        self.icone = self.icone.resize((45, 40), Image.ANTIALIAS)
        self.icone = ImageTk.PhotoImage(self.icone)

        self.imagem_dado = Image.open(r"C:\Users\sotam\OneDrive\Desktop\Códigos\Cassino\dado.png")  # Coloque o caminho da imagem do dado aqui
        self.imagem_dado = self.imagem_dado.resize((100, 100), Image.ANTIALIAS)
        self.imagem_dado = ImageTk.PhotoImage(self.imagem_dado)

        # Criar componente ícone de home
        self.button3 = tk.Button(self.master, image= self.icone, relief=FLAT, command=self.abrir_menu, bg='#228b22')
        self.button3.pack(anchor='nw', padx=1, pady=1)

        self.label_imagem = tk.Label(self.master, image=self.imagem_dado, bg='#228b22')
        self.label_imagem.pack(pady=1)

        self.label_resultado = tk.Label(self.master, text="", font=("Helvetica", 48), bg='#228b22')
        self.label_resultado.pack(pady=8)
        
        self.botao_lancar = tk.Button(self.master, text="Lançar Dado", command=self.rolar_dados, bg='#cc0000', font=("Helvetica",12,'bold'))
        self.botao_lancar.pack()

    def rolar_dados(self):
            # Atualiza a imagem do dado para simular o giro
            for _ in range(10):
                resultado = randint(1, 6)
                self.label_resultado["text"] = str(resultado)
                self.master.update()
                time.sleep(0.3)
    
    def abrir_menu(self):
         self.master.withdraw()
         root.deiconify()

if __name__=='__main__':
    root = tk.Tk()
    app = Inicio(root)
    root.mainloop()
