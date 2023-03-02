# pdv-em-python
Sistema pdv em python


import tkinter as tk
import sqlite3

class PDV:
    def __init__(self, master):
        self.master = master
        self.master.title("PDV")
        self.master.geometry("1000x800")
        
        # criar uma conexão com o banco de dados
        self.conn = sqlite3.connect('pdv.db')
        self.c = self.conn.cursor()
        
        # criar tabela de produtos se não existir
        self.c.execute('''CREATE TABLE IF NOT EXISTS produtos
                          (id INTEGER PRIMARY KEY AUTOINCREMENT,
                           nome TEXT,
                           preco REAL)''')
        self.conn.commit()
        
        # criar widgets da interface gráfica
        self.label1 = tk.Label(self.master, text="Produto:")
        self.label1.pack()
        self.entry1 = tk.Entry(self.master)
        self.entry1.pack()
        
        self.label2 = tk.Label(self.master, text="Preço:")
        self.label2.pack()
        self.entry2 = tk.Entry(self.master)
        self.entry2.pack()
        
        self.button1 = tk.Button(self.master, text="Adicionar Produto", command=self.adicionar_produto)
        self.button1.pack()
        
        self.button2 = tk.Button(self.master, text="Ver Produtos", command=self.ver_produtos)
        self.button2.pack()
        
        self.button3 = tk.Button(self.master, text="Fechar", command=self.fechar)
        self.button3.pack()
    
    def adicionar_produto(self):
        # inserir produto na tabela de produtos
        nome = self.entry1.get()
        preco = self.entry2.get()
        self.c.execute("INSERT INTO produtos (nome, preco) VALUES (?, ?)", (nome, preco))
        self.conn.commit()
        self.entry1.delete(0, tk.END)
        self.entry2.delete(0, tk.END)
    
    def ver_produtos(self):
        # obter produtos da tabela de produtos e exibi-los em uma nova janela
        produtos = self.c.execute("SELECT * FROM produtos").fetchall()
        produtos_window = tk.Toplevel(self.master)
        for produto in produtos:
            tk.Label(produtos_window, text=produto[1]).pack()
            tk.Label(produtos_window, text=produto[2]).pack()
    
    def fechar(self):
        # fechar a conexão com o banco de dados e sair do programa
        self.conn.close()
        self.master.destroy()

root = tk.Tk()
pdv = PDV(root)
root.mainloop()
