# Aufgabenblatt 1

## (1) Abwechselnd Hoch- und Runterzählen

```csharp
internal class Program {
    static void Main(string[] args) {
        int n = 16;

        for (int i = 1; i <= n / 2; i++) {
            Console.Write(i);
            Console.Write(" ");
            Console.Write(n - i + 1);
            Console.Write(" ");
        }
    }
}
```

## (2): FizzBuzz

```csharp
internal class Program {
    static void Main(string[] args) {
        // Ask the user for a positive integer.
        Console.Write("Please enter a positive integer: ");
        int max = int.Parse(Console.ReadLine());

        // Iterate all numbers from 1 to max.
        for (int i  = 1; i <= max; i++) {
            // Print depending on divisibility.
            if (i % 5 == 0 && i % 3 == 0) {
                Console.WriteLine("FizzBuzz");
            }
            else if (i % 3 == 0) {
                Console.WriteLine("Fizz");
            }
            else if (i % 5 == 0) {
                Console.WriteLine("Buzz");
            }
            else {
                Console.WriteLine(i);
            }
        }
    }
}
```

## (3) Codeknacker

```csharp
internal class Program {
    static void Main(string[] args) {
        // Generate the secret.
        int secretNumber = Random.Shared.Next(1, 100);
        Console.WriteLine("Die Zufallszahl wurde generiert. Viel Glück!");

        // Let the player guess until they find the secret number.
        while (true) {
            // Get next guess from the player.
            Console.Write("Ihr Versuch: ");
            int guess = int.Parse(Console.ReadLine());

            // Check if the player has won.
            if (guess == secretNumber) {
                Console.WriteLine($"Gratulation, Sie haben gewonnen! Die gesuchte Zahl war {secretNumber}.");
                break;
            }
            
            // Otherwise, give the player a tip.
            if (secretNumber < guess) {
                Console.WriteLine("Die gesuchte Zahl ist kleiner.");
            }
            else {
                Console.WriteLine("Die gesuchte Zahl ist größer.");
            }
        }
    }
}
```

## (4) Hex-"Editor"

```csharp
class Program {
    static void Main(string[] args) {
        // Get path.
        Console.Write("Which file do you want to open? ");
        string path = Console.ReadLine()!;

        // Check if file exists.
        if (!File.Exists(path)) {
            Console.WriteLine($"ERROR: File {path} does not exist.");
            return;
        }

        // Read file.
        byte[] data = File.ReadAllBytes(path);

        // Print hex view.
        Console.WriteLine("\n          00 01 02 03 04 05 06 07 08 09 0A 0B 0C 0D 0E 0F\n");

        int pos = 0;
        while (pos < data.Length) {
            // Print line index.
            Console.Write(pos.ToString("X8") + "  ");

            // Print line in hex.
            for (int i = pos; i < data.Length && i < pos + 16; i++) {
                Console.Write(data[i].ToString("X2") + " ");
            }

            Console.Write(" ");

            // Add more spacing in front if this is the last line and
            // it is incomplete (less than 16 bytes).
            if (data.Length - pos < 16) {
                int spaces = 16 - (data.Length - pos);

                for (int i = 0; i < spaces; i++) {
                    Console.Write("   ");
                }
            }

            // Print line as text.
            for (int i = pos; i < data.Length && i < pos + 16; i++) {
                char text = (char)data[i];

                if (char.IsControl(text)) {
                    text = '.';
                }

                Console.Write(text);
            }

            // Line break.
            Console.WriteLine();

            // Bump pos.
            pos += 16;
        }
    }
}
```