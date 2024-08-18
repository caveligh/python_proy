import random

class Ship:
    def __init__(self, name, size):
        self.name = name
        self.size = size
        self.positions = []
        self.hits = 0

    def place_ship(self, start_row, start_col, direction, board):
        positions = []
        if direction == 'H':
            if start_col + self.size > len(board[0]):
                return False
            for i in range(self.size):
                if board[start_row][start_col + i] != ' ':
                    return False
                positions.append((start_row, start_col + i))
        elif direction == 'V':
            if start_row + self.size > len(board):
                return False
            for i in range(self.size):
                if board[start_row + i][start_col] != ' ':
                    return False
                positions.append((start_row + i, start_col))
        else:
            return False
        
        for pos in positions:
            board[pos[0]][pos[1]] = self.name[0]
        self.positions = positions
        return True

    def hit(self):
        self.hits += 1
        return self.hits == self.size

class Destroyer(Ship):
    def __init__(self):
        super().__init__('Destructor', 2)

class Submarine(Ship):
    def __init__(self):
        super().__init__('Submarino', 3)

class Battleship(Ship):
    def __init__(self):
        super().__init__('Acorazado', 4)

class Player:
    def __init__(self, name, is_computer=False):
        self.name = name
        self.board = [[' ' for _ in range(10)] for _ in range(10)]
        self.ships = []
        self.hits = [[' ' for _ in range(10)] for _ in range(10)]
        self.is_computer = is_computer

    def place_ships(self):
        ships = [Destroyer(), Submarine(), Battleship()]
        for ship in ships:
            while True:
                if self.is_computer:
                    start_row = random.randint(0, 9)
                    start_col = random.randint(0, 9)
                    direction = random.choice(['H', 'V'])
                else:
                    print(f"{self.name}, coloca tu {ship.name} de tamaño {ship.size}.")
                    start_row = int(input("Fila inicial (0-9): "))
                    start_col = int(input("Columna inicial (0-9): "))
                    direction = input("Dirección (H para horizontal, V para vertical): ").upper()
                if ship.place_ship(start_row, start_col, direction, self.board):
                    self.ships.append(ship)
                    if not self.is_computer:
                        self.print_board(self.board)
                    break
                elif not self.is_computer:
                    print("Posición no válida. Inténtalo de nuevo.")

    def print_board(self, board):
        print("  0 1 2 3 4 5 6 7 8 9")
        for i, row in enumerate(board):
            print(f"{i} {' '.join(row)}")
        print()

    def attack(self, opponent):
        while True:
            if self.is_computer:
                row = random.randint(0, 9)
                col = random.randint(0, 9)
            else:
                print(f"{self.name}, elige una posición para atacar.")
                row = int(input("Fila (0-9): "))
                col = int(input("Columna (0-9): "))
            if 0 <= row < 10 and 0 <= col < 10 and self.hits[row][col] == ' ':
                if opponent.board[row][col] == ' ':
                    print("Agua!")
                    self.hits[row][col] = 'A'
                    opponent.board[row][col] = 'A'
                    break
                else:
                    print("Impacto!")
                    self.hits[row][col] = 'T'
                    for ship in opponent.ships:
                        if (row, col) in ship.positions:
                            if ship.hit():
                                print(f"¡Hundido! Has hundido el {ship.name} de {opponent.name}.")
                            break
                    opponent.board[row][col] = 'T'
                    break
            elif not self.is_computer:
                print("Posición no válida o ya atacada. Intenta de nuevo.")

    def all_ships_sunk(self):
        return all(ship.hits == ship.size for ship in self.ships)

class BattleshipGame:
    def __init__(self):
        self.player1 = Player("Jugador 1")
        self.player2 = Player("Computadora", is_computer=True)

    def play(self):
        print("Bienvenido al juego de Batalla Naval!")
        print("Jugador 1 coloca sus barcos.")
        self.player1.place_ships()
        print("La computadora está colocando sus barcos...")
        self.player2.place_ships()

        current_player = self.player1
        opponent = self.player2

        while True:
            if not current_player.is_computer:
                print("\nTu tablero:")
                self.player1.print_board(self.player1.board)
                print("\nTus ataques:")
                self.player1.print_board(self.player1.hits)

            current_player.attack(opponent)
            
            if opponent.all_ships_sunk():
                print(f"\n¡{current_player.name} ha ganado el juego!")
                break
            
            current_player, opponent = opponent, current_player

# Crear una instancia del juego y jugar
game = BattleshipGame()
game.play()