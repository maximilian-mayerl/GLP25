# Übungsblatt 2


## (1) Calculator Revisited

```csharp
namespace Calculator {
    internal class Program {
        private static int GetNumberOfValues() {
            while (true) {
                Console.Write("Wie viele Zahlen wollen Sie eingeben? ");
                
                int numberOfInputs;
                bool success = int.TryParse(Console.ReadLine(), out numberOfInputs);

                // We are done when the users makes a legal input.
                if (success && numberOfInputs >= 1) {
                    return numberOfInputs;
                }

                Console.WriteLine("Bitte geben Sie eine Anzahl >= 1 ein.");
            }
        }

        private static double ReadValueForIndex(int index) {
            double value;

            do {
                Console.Write("Number #" + index + ": ");

            } while (!double.TryParse(Console.ReadLine(), out value));

            return value;
        }
        private static void ReadAndFillValues(double[] numbers) {
            for (int i = 0; i < numbers.Length; i++) {
                numbers[i] = ReadValueForIndex(i + 1);
            }
        }

        private static string GetOperation() {
            while (true) {
                Console.Write("Welche Operation wollen Sie ausführen (+, -, *): ");
                string operation = Console.ReadLine();

                // We are done when the users makes a legal input.
                if (operation == "+" || operation == "-" || operation == "*") {
                    return operation;
                }

                Console.WriteLine("Bitte geben Sie eine erlaubte Operation ein.");
            }
        }

        private static double CalculateOperation(string operation, double[] numbers) {
            // We start with the first number that the user has input.
            double result = numbers[0];

            // Now, loop through the remaining inputs and perform the specified operation.
            // Note that we start from the second input here (i == 1), because we already 
            // handled the first input as the initial value of `result`.
            for (int i = 1; i < numbers.Length; i++) {
                switch (operation) {
                    case "+":
                        result += numbers[i];
                        break;
                    case "-":
                        result -= numbers[i];
                        break;
                    case "*":
                        result *= numbers[i];
                        break;
                }
            }

            // Done.
            return result;
        }

        static void Main(string[] args) {
            // Repeat everything so the user can do multiple calculations without having the restart the program.
            while (true) {
                // Ask user how many numbers they want to calculate with.
                int numberOfInputs = GetNumberOfValues();

                // Create array to hold the inputs
                double[] numbers = new double[numberOfInputs];

                // Read inputs from the user and store then in the array.
                ReadAndFillValues(numbers);

                // Ask user what operation they want to calculate with.
                string operation = GetOperation();

                // Perform the calculation. 
                double result = CalculateOperation(operation, numbers);

                // Output result.
                Console.WriteLine("Result: " + result);
            }
        }
    }
}
```

## Array-Library

```csharp
namespace ArrayLibrary {
    class IntArrayTools {
        public static double Average(int[] input) {
            double sum = 0;

            foreach (int value in input) {
                sum += value;
            }

            return sum / input.Length;
        }

        public static int Min(int[] input) {
            int min = input[0];

            foreach (int value in input) {
                if (value < min) {
                    min = value;
                }
            }

            return min;
        }

        public static int Max(int[] input) {
            int max = input[0];

            foreach (int value in input) {
                if (value > max) {
                    max = value;
                }
            }

            return max;
        }

        public static int[] Concat(int[] left, int[] right) {
            int[] result = new int[left.Length + right.Length];

            // Copy the left array to the result.
            for (int i = 0; i < left.Length; i++) {
                result[i] = left[i];
            }

            // Copy the right array to the result.
            for (int i = 0; i < right.Length; i++) {
                result[i + left.Length] = right[i];
            }

            return result;
        }

        public static int[] Reverse(int[] input) {
            int[] result = new int[input.Length];

            // Copy elements in reverse order.
            for (int i = 0; i < input.Length; i++) {
                result[i] = input[input.Length - i - 1];
            }

            return result;
        }
    }

    internal class Program {

        static void Main(string[] args) {
            int[] firstArray = new int[] { 1, 2, 3 };
            int[] secondArray = new int[] { 4, 6, 8 };

            double average = IntArrayTools.Average(firstArray);
            Console.WriteLine($"Average is {average}");

            double min = IntArrayTools.Min(firstArray);
            Console.WriteLine($"Min is {min}");

            double max = IntArrayTools.Max(firstArray);
            Console.WriteLine($"Max is {max}");

            int[] merged = IntArrayTools.Concat(firstArray, secondArray);
            Console.WriteLine("Concatenation is { " + string.Join(", ", merged) + " }");

            int[] reversed = IntArrayTools.Reverse(firstArray);
            Console.WriteLine("Reversed is { " + string.Join(", ", reversed) + " }");
        }
    }
}
```

## Tic-Tac-Toe

```csharp
namespace TicTacToe {
    internal class Program {
        private static char[,] InitializePlayingField() {
            // To begin with, we will they playing field with spaces.
            return new char[,] {
                { ' ', ' ', ' ' },
                { ' ', ' ', ' ' },
                { ' ', ' ', ' ' },
            };
        }

        private static int AskPosition(string positionType) {
            // Keep asking the user for input until they give a legal number.
            while (true) {
                Console.Write($"In which {positionType} would you like to place your symbol? ");

                int position;
                bool success = int.TryParse(Console.ReadLine(), out position);

                // Return if the used entered a legal position.
                if (success && position >= 0 && position < 3) {
                    return position;
                }

                // Otherwise, report an appropriate error and let the user try again.
                Console.WriteLine("Please inpout a legal numeric value between 0 and 2.");
            }
        }

        private static void PlayPlayerTurn(char currentPlayer, char[,] field) {
            // Keep asking the user for a target position until they give an available one.
            while (true) {
                // Get target position.
                int row = AskPosition("row");
                int column = AskPosition("column");

                // Check if that spot is still available.
                // If yes, place the symbol and we are done here.
                if (field[row, column] == ' ') {
                    field[row, column] = currentPlayer;
                    return;
                }

                // Otherwise, report an error and let the user try again.
                Console.WriteLine("This spot is already taken. Please try again ...");
            }
        }

        private static bool IsGameOver(char[,] field) {
            // There are eight possible configurations of the player field where the game is over.
            // You could do this with loops, but since the number of configurations is so small,
            // we can also just write them out.
            return
                (field[0, 0] != ' ' && field[0, 0] == field[0, 1] && field[0, 1] == field[0, 2])        // First row.
                || (field[1, 0] != ' ' && field[1, 0] == field[1, 1] && field[1, 1] == field[1, 2])     // Second row.
                || (field[2, 0] != ' ' && field[2, 0] == field[2, 1] && field[2, 1] == field[2, 2])     // Third row.
                || (field[0, 0] != ' ' && field[0, 0] == field[1, 0] && field[1, 0] == field[2, 0])     // First column.
                || (field[0, 1] != ' ' && field[0, 1] == field[1, 1] && field[1, 1] == field[2, 1])     // First column.
                || (field[0, 2] != ' ' && field[0, 2] == field[1, 2] && field[1, 2] == field[2, 2])     // First column.
                || (field[0, 0] != ' ' && field[0, 0] == field[1, 1] && field[1, 1] == field[2, 2])     // First diagonal.
                || (field[0, 2] != ' ' && field[0, 2] == field[1, 1] && field[1, 1] == field[2, 0]);    // Second diagonal.
        }

        private static void PrintPlayingField(char[,] field) {
            Console.WriteLine();
            Console.WriteLine($" {field[0, 0]} | {field[0, 1]} | {field[0, 2]}");
            Console.WriteLine("-----------");
            Console.WriteLine($" {field[1, 0]} | {field[1, 1]} | {field[1, 2]}");
            Console.WriteLine("-----------");
            Console.WriteLine($" {field[2, 0]} | {field[2, 1]} | {field[2, 2]}");
            Console.WriteLine();
        }

        static void Main(string[] args) {
            // Create the playing field. We use a char array for this.
            char[,] field = InitializePlayingField();

            // Main game loop.
            char currentPlayer = 'O';

            while (true) {
                // Give turn info and let the player play their turn.
                Console.WriteLine($"It's {currentPlayer}'s turn.");
                PlayPlayerTurn(currentPlayer, field);

                // Draw the playing field.
                PrintPlayingField(field);

                // Check if the game is over.
                if (IsGameOver(field)) {
                    // The winner has to be the current player.
                    Console.WriteLine($"Game over! {currentPlayer} wins the game!");
                    break;
                }

                // Switch player.
                currentPlayer = (currentPlayer == 'O') ? 'X' : 'O';
            }
        }
    }
}

```
