from datetime import date

class Especie:
    def __init__(self, descricao):
        self.descricao = descricao

class Area:
    def __init__(self, referencia, descricao):
        self.referencia = referencia
        self.descricao = descricao
        self.animais = []

    def adicionar_animal(self, animal):
        self.animais.append(animal)

class Animal:
    def __init__(self, nome, num_chip, data_nascimento, especie, area):
        self.nome = nome
        self.num_chip = num_chip
        self.data_nascimento = data_nascimento
        self.especie = especie
        self.area = area
        # Adiciona automaticamente o animal à lista da área correspondente
        area.adicionar_animal(self)

class Visitante:
    def __init__(self, nome, email, nif):
        self.nome = nome
        self.email = email
        self.nif = nif
        self.areas_visitadas = []

    def visitar_area(self, area):
        if area not in self.areas_visitadas:
            self.areas_visitadas.append(area)

class Zoo:
    def __init__(self):
        self.visitantes = {} # Usamos um dicionário com NIF como chave para validação rápida

    def registar_visitante(self, nome, email, nif):
        if nif in self.visitantes:
            print(f"Aviso: O visitante com CC {nif} já está registado.")
            return self.visitantes[nif]
        else:
            novo_visitante = Visitante(nome, email, nif)
            self.visitantes[nif] = novo_visitante
            print(f"Visitante {nome} registado com sucesso.")
            return novo_visitante

    def mostrar_roteiro(self, nif):
        visitante = self.visitantes.get(nif)
        if not visitante:
            print("Visitante não encontrado.")
            return

        print(f"\n--- Roteiro de Visita: {visitante.nome} ---")
        if not visitante.areas_visitadas:
            print("Nenhuma área visitada ainda.")
        
        for area in visitante.areas_visitadas:
            print(f"\nÁrea: {area.descricao} (Ref: {area.referencia})")
            print("Animais nesta área:")
            if not area.animais:
                print("  - De momento, não há animais nesta área.")
            for animal in area.animais:
                print(f"  - {animal.nome} | Espécie: {animal.especie.descricao} | Chip: {animal.num_chip}")

# --- Teste do Sistema ---

# 1. Criar Espécies e Áreas
savana = Area("S01", "Savana Africana")
reptilario = Area("R01", "Reptilário Central")

leao_sp = Especie("Leão")
cobra_sp = Especie("Cobra Piton")

# 2. Criar Animais
simba = Animal("Simba", 12345, date(2020, 5, 10), leao_sp, savana)
ka = Animal("Ka", 67890, date(2018, 3, 15), cobra_sp, reptilario)

# 3. Gerir Zoo e Visitantes
meu_zoo = Zoo()

# Registar visitante (e tentar registar repetido)
v1 = meu_zoo.registar_visitante("João Silva", "joao@email.com", "12345678")
v1_repetido = meu_zoo.registar_visitante("João Silva", "joao@email.com", "12345678")

# 4. Registar Visitas
v1.visitar_area(savana)
v1.visitar_area(reptilario)

# 5. Mostrar Informação (Ponto 6 do exercício)
meu_zoo.mostrar_roteiro("12345678")
