# EsfolCalc
Данный проект представляет калькулятор в столбик, выполненный в коносли на языке c#. 

Код:

using System;

namespace EfsolCalc

{

    class Program
    
    {
    
        static void Main(string[] args)
        
        {
        
            double firstNumb = InputDigit(1);
            string oper = OperationInput();
            double secondNumb = InputDigit(2);
            switch (oper)
            {
                case "+":
                    Addition(firstNumb, secondNumb);
                    break;

                case "-":
                    Subtraction(firstNumb, secondNumb);
                    break;

                case "*":
                    Multiplication(firstNumb, secondNumb);
                    break;

                case "/":
                    Division(firstNumb, secondNumb);
                    break;
            }
            Console.ReadLine();
        }
        static public double InputDigit(int digit)
        {
            Console.WriteLine($"Введите {digit} число:");
            string inp = Console.ReadLine();
            bool test = double.TryParse(inp, out double g);
            if (test == true)
            {
                double numb = g;
                return numb;
            }
            else
            {
                Console.WriteLine("Вы некорректно число! Ну что же, попробуем еще раз!");
                return InputDigit(digit);
            }
        }
        static public string OperationInput()
        {
            Console.WriteLine("Введите оператор:");
            string enter = Console.ReadLine();
            if (enter == "+" || enter == "-" || enter == "*" || enter == "/")
            {
                string oper = enter;
                return oper;
            }
            else
            {
                Console.WriteLine("Вы некорректно ввели оператор, попробуйте еще раз. Мы верим, все получится!");
                return OperationInput();
            }
        }
        static public int DigitLength(double digit)
        {
            string stringDigit = digit.ToString();
            int DigitLength = 0;
            for (int i = 0; i < stringDigit.Length; i++)
            {
                if (stringDigit[i] != ',' && stringDigit[i] != '-')
                {
                    DigitLength += 1;
                }
            }
            return DigitLength;
        }
        static public int FractionLength(double digit)
        {
            string stringDigit = digit.ToString();
            if (stringDigit.Contains(",")) 
            {
                string[] digitArray = stringDigit.Split(",");
                int FractionLength = 0;
                for (int i = 0; i < digitArray[1].Length; i++)
                {
                    FractionLength += 1;
                }
                return FractionLength; 
            }
            else
            {
                return 0;
            }
        }
        static public int NoFractLength(double digit)
        {
            string stringDigit = digit.ToString();
            if (stringDigit.Contains(","))
            {
                string[] digitArray = stringDigit.Split(",");
                int noFractionLength = 0;
                for (int i = 0; i < digitArray[0].Length; i++)
                {
                    noFractionLength += 1;
                }
                return noFractionLength;
            }
            else
            {
                return 1;
            }
        }
        static public int Biggestlength(int first, int second, int result)
        {
            int biggestLength = 0;

            if (result > first + 1 && result > second)
            {
                biggestLength = result;
                return biggestLength;
            }

            else if (first + 1 > second)
            {
                biggestLength = first + 1;
                return biggestLength;
            }

            else
            {
                biggestLength = second;
                return biggestLength;
            }
        }
        static public int Biggestlength(int first, int second)
        {
            int biggestLength = 0;
            if (first + 1 > second)
            {
                biggestLength = first + 1;
                return biggestLength;
            }

            else
            {
                biggestLength = second;
                return biggestLength;
            }
        }
        static public string CreateLine (int length)
        {
            string line = "";
            for (int i = 0; i < length; i++)
            {
                line += "_";
            }
            return line;
        }
        static public string CreateShift(int length)
        {
            string shift = "";
            for (int i = 0; i < length; i++)
            {
                shift += " ";
            }
            return shift;
        }
        public static string ReverseString(string s)
        {
            char[] arr = s.ToCharArray();
            Array.Reverse(arr);
            return new string(arr);
        }
        static public int DigitSumm (string digitString, int position, int increseDigit)
        {
            double Summ = 0;
            string digitS = increseDigit.ToString();
            char[] summArr = new char [digitS.Length + 1];
            for (int i = 0; i < digitS.Length; i++)
            {
                summArr[i] = digitS[i];
            }
            summArr[digitS.Length] = digitString[position];

            string I = new string(summArr);
            Summ = Convert.ToInt32(I);   
            return (int)Summ;
        }
        static public string AddZero (string digitString)
        {
            string digitString2 = digitString + "0";
            return digitString2;
        }
        static public void Addition(double digit, double digit_2)
        {
            double big = 0;
            double small = 0;

            if (digit > digit_2)
            {
                big = digit;
                small = digit_2;
            }
            else
            {
                big = digit_2;
                small = digit;
            }

            string bigS = big.ToString();
            string smallS = small.ToString();

            int bigFract;
            int smallFract;
            int biggestFract = 0;
            if (smallS.Contains(",") || bigS.Contains(","))
            {
                smallFract = FractionLength(small);
                bigFract = FractionLength(big);
                if (smallFract > bigFract)
                {
                    biggestFract = smallFract;
                }
                else
                {
                    biggestFract = bigFract;
                }
            }

            small *= Math.Pow(10, biggestFract);
            big *= Math.Pow(10, biggestFract);

            double result;
            result = big + small;

            int resultLength = DigitLength(result);
            int bigLength = DigitLength(big);
            int smallLength = DigitLength(small);
            int greatLength = Biggestlength(bigLength, smallLength, resultLength);

            string bigOutput = CreateShift(greatLength - bigLength - 1);
            string smallOutput = CreateShift(greatLength - smallLength);
            string resultOutput = CreateShift(greatLength - resultLength);
            string line = CreateLine(greatLength);

            Console.WriteLine($"+{bigOutput}{big / Math.Pow(10, biggestFract)}");
            Console.WriteLine($"{smallOutput}{small/ Math.Pow(10, biggestFract)}");
            Console.WriteLine($"{line}");
            Console.WriteLine($"{resultOutput}{result/ Math.Pow(10, biggestFract)}");
         }
        static public void Subtraction(double digit, double digit_2)
        {
            string digitS = digit.ToString();
            string digit_2S = digit_2.ToString();

            int digitFract = 0;
            int digit_2Fract = 0;
            int biggestFract = 0;
            if (digitS.Contains(",") || digit_2S.Contains(","))
            {
                digit_2Fract = FractionLength(digit_2);
                digitFract = FractionLength(digit);
                if (digit_2Fract > digitFract)
                {
                    biggestFract = digit_2Fract;
                }
                else
                {
                    biggestFract = digitFract;
                }
            }

            digit = digit * Math.Pow(10, biggestFract);
            digit_2 = digit_2 * Math.Pow(10, biggestFract);

            double result;
            result = digit - digit_2;

            int resultLength = DigitLength(result);
            int digitLength = DigitLength(digit);
            int digit_2Length = DigitLength(digit_2);
            int greatLength = Biggestlength(digitLength, digit_2Length, resultLength);
            
            string digitOutput = CreateShift(greatLength - (digitLength + 1)); 
            string digit_2Output = CreateShift(greatLength - digit_2Length);
            string resultOutput = CreateShift(greatLength - resultLength);
            string line = CreateLine(greatLength);

            Console.WriteLine($"-{digitOutput}{digit / Math.Pow(10, biggestFract)}");
            Console.WriteLine($"{digit_2Output}{digit_2 / Math.Pow(10, biggestFract)}");
            Console.WriteLine($"{line}");
            Console.WriteLine($"{resultOutput}{result / Math.Pow(10, biggestFract)}");
        }
        static public void Multiplication(double digit, double digit_2)
        {
            double big;
            double small;

            if (digit > digit_2)
            {
                big = digit;
                small = digit_2;
            }
            else
            {
                big = digit_2;
                small = digit;
            }

            int bigFract = FractionLength(big);
            int smallFract = FractionLength(small);

            string bigS = big.ToString();
            string smallS = small.ToString();

            big = big * Math.Pow(10, bigFract);
            small = small * Math.Pow(10, smallFract);

            double result = 0;
            result = big * small;

            int resultLength = DigitLength(result);
            int bigLength = DigitLength(big);
            int smallLength = DigitLength(small);
            int biggestOfbigLength = Biggestlength(bigLength, smallLength);
            int greatLength = Biggestlength(bigLength, smallLength, resultLength);

            string bigOutput = CreateShift (greatLength - bigLength - 1);
            string smallOutput =CreateShift (greatLength - smallLength);
            string resultOutput = CreateShift (greatLength - resultLength);
            string topLine = CreateLine(biggestOfbigLength);
            string bottomLine = CreateLine(greatLength);
            
            Console.WriteLine($"*{bigOutput}{big / Math.Pow(10, bigFract)}");
            Console.WriteLine($"{smallOutput}{small / Math.Pow(10, smallFract)}");

            int shiftLength = resultLength - smallLength;

            if (DigitLength (small) > 1)
            {
                Console.WriteLine($"{smallOutput}{topLine}");
                for (int i = 0; i < smallS.Length; i++)
                {
                    double multiPart = big * (smallS[smallS.Length - (i + 1)] - '0');
                    if (multiPart <= 0)
                    {
                        continue;
                    }
                    if (DigitLength(multiPart) > smallLength)
                    {
                        i++;
                    }
                    string shift = "";
                    int antiShift = shiftLength - i;
                    for (int j = 0; j < antiShift; j++)
                    {
                        shift += " ";
                    }
                    if (DigitLength(multiPart) > smallLength)
                    {
                        i--;
                    }
                    Console.WriteLine($"{shift}{multiPart}");
                }
            }         
            Console.WriteLine($"{bottomLine}");
            Console.WriteLine($"{resultOutput}{result / Math.Pow(10, (bigFract + smallFract))}");
        }
        static public void Division (double digit, double digit_2)
        {
            string digitS = digit.ToString();
            string digit_2S = digit_2.ToString();
            int digitFract = 0;
            int digit_2Fract = 0;
            int biggestFract = 0;
            if (digitS.Contains(",") || digit_2S.Contains(","))
            {
                digitFract = FractionLength(digit);
                digit_2Fract = FractionLength(digit_2);
                if (digit_2Fract > digitFract)
                {
                    biggestFract = digit_2Fract;
                }
                else
                {
                    biggestFract = digitFract;
                }
            }
            digit = digit * Math.Pow(10, biggestFract);
            digit_2 = digit_2 * Math.Pow(10, biggestFract);


            string shift = "";
            string line = "";
            int currentDivided = (int)(digitS[0] - '0');
            double result = 0;
            int residue;
            int min;
            digitS = digit.ToString();
            digit_2S = digit_2.ToString();


            for (int i = 0; ; i++)
            {
                if ((currentDivided / (int)digit_2) > 0)
                {
                    line = CreateLine((int)Math.Log10(digit_2 + currentDivided) + 1);
                    residue = (int)currentDivided % (int)digit_2;
                    result += (currentDivided / (int)digit_2) * Math.Pow(10, digitS.Length - (i+1));
                    min = ((int)(digit_2 * ((currentDivided/(int)digit_2))));
                    Console.WriteLine($"{shift}{min}");
                    Console.WriteLine($"{shift}{line}");
                    Console.WriteLine($"{shift + CreateShift( DigitLength(min) - 1)}{residue}");
                    currentDivided = (residue);
                    if (residue == 0)
                    {
                        break;
                    }
                }
                else
                {
                    if (i+1 < digitS.Length)
                    {
                        currentDivided = DigitSumm(digitS, i + 1, currentDivided);
                    }
                    else
                    {
                        digitS = AddZero(digitS);
                        i--;
                    }
                }
                shift += " ";
            }
            Console.WriteLine (result / Math.Pow(10, biggestFract));
        }
    }
}


Скриншоты:
0) Проверка ввода

0.1) Пользователь некорректно ввел число
![Plus1](https://github.com/Duck27/EsfolCalc/blob/master/FFFFFFFF.png)

0.2)Пользователь некорректно ввел оператор
![Plus1](https://github.com/Duck27/EsfolCalc/blob/master/EEEEEEE.png)



1) Сложение

1.1) Сложение чисел длиной в одну цифру
![Plus1](https://github.com/Duck27/EsfolCalc/blob/master/1е.png)

1.2) Сложение числа длиной в две цифры и в одну
![Plus1](https://github.com/Duck27/EsfolCalc/blob/master/2е.png)

1.3) Сложение чисел длиной в две цифры 
![Plus1](https://github.com/Duck27/EsfolCalc/blob/master/3е.png)

1.4) Сложение дробного числа с целым
![Plus1](https://github.com/Duck27/EsfolCalc/blob/master/4е.png)

1.5) Сложение дробных чисел
![Plus1](https://github.com/Duck27/EsfolCalc/blob/master/5e.png)

1.6) Сложение с отрицательным числом
![Plus1](https://github.com/Duck27/EsfolCalc/blob/master/6e.png)



2) Вычитание

2.1) Вычитание чисел длиной в одну цифру
![Plus1](https://github.com/Duck27/EsfolCalc/blob/master/1t.png)

2.2) Вычитание чисел длиной в две цифры и одну
![Plus1](https://github.com/Duck27/EsfolCalc/blob/master/2t.png)

2.3) Вычитание чисел длиной в три цифры
![Plus1](https://github.com/Duck27/EsfolCalc/blob/master/3t.png)

2.4) Вычитание дробных чисел
![Plus1](https://github.com/Duck27/EsfolCalc/blob/master/4t.png)

2.5) Вычитание дробных чисел (2)
![Plus1](https://github.com/Duck27/EsfolCalc/blob/master/5t.png)

2.6) Вычитание с отрицательным числом
![Plus1](https://github.com/Duck27/EsfolCalc/blob/master/6t.png)



3) Умножение 

3.1)Умножение чисел длиной в одну цифру
![Plus1](https://github.com/Duck27/EsfolCalc/blob/master/r1.png)

3.2) Умножение чисел длиной в две цифры и одну
![Plus1](https://github.com/Duck27/EsfolCalc/blob/master/2r.png)

3.3) Умножение чисел длиной в три цифры и две
![Plus1](https://github.com/Duck27/EsfolCalc/blob/master/3r.png)

3.4) Умножение дробных чисел
![Plus1](https://github.com/Duck27/EsfolCalc/blob/master/4r.png)



4) Деление 

4.1) Деление чисел
![Plus1](https://github.com/Duck27/EsfolCalc/blob/master/uf1.png)

4.2) Деление больших чисел
![Plus1](https://github.com/Duck27/EsfolCalc/blob/master/uf2.png)






