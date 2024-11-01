Файл Main:
public class Main {
    public static void main(String[] args)
    {
        // Создаем экземпляры классов
        TextUtils textUtils = new TextUtils("тест");
        ArrayUtils arrayUtils = new ArrayUtils(new int[]{1,2,3});

        // Используем методы класса TextUtils для работы с текстом
        int sentenceCount = textUtils.countSentence();
        System.out.println("Число предложений в тексте: " + sentenceCount);

        String word = textUtils.mostFreqntWord();
        System.out.println("Самое часто встречающееся слово: " + word);

        int vowelCount = textUtils.firstLetrVowel();
        int consonantCount = textUtils.firstLetrConsonant();
        System.out.println("Число слов, начинающихся с гласной: " + vowelCount);
        System.out.println("Число слов, начинающихся с согласной: " + consonantCount);

        // Клонируем экземпляры
        TextUtils clonedTextUtils = textUtils.clone();
        ArrayUtils clonedArrayUtils = arrayUtils.clone();

        // Используем метод класса ArrayUtils для работы с массивом
        int[] uniqueNumbers = arrayUtils.removeDuplicates();
        System.out.println("Массив без повторений:");
        for (int number : uniqueNumbers)
        {
            System.out.print(number + " ");
        }
        System.out.println();

        // Выводим клонированный текст и массив
        System.out.println("Клонированный текст: " + clonedTextUtils.getText());
        int[] clonedUniqueNumbers = clonedArrayUtils.removeDuplicates();
        System.out.println("Клонированный массив без повторений:");
        for (int number : clonedUniqueNumbers)
        {
            System.out.print(number + " ");
        }
        System.out.println();
    }
}
Файл ArrayUtils:
import java.util.Arrays;

public class ArrayUtils implements Cloneable
{
    private int[] numbers;

    public ArrayUtils(int[] numbers)
    {
        this.numbers = numbers;
    }

    public int[] removeDuplicates()
    {
        int[] uniqueNumbers = new int[numbers.length];
        int count = 0;

        for (int i = 0; i < numbers.length; i++)
        {
            boolean isDuplicate = false;

            for (int j = 0; j < i; j++)
            {
                if (numbers[i] == numbers[j])
                {
                    isDuplicate = true;
                    break;
                }
            }

            if (!isDuplicate)
            {
                uniqueNumbers[count++] = numbers[i];
            }
        }

        return Arrays.copyOf(uniqueNumbers, count);
    }

    public ArrayUtils clone()
    {
        try {
            ArrayUtils cloned = (ArrayUtils) super.clone();
            cloned.numbers = numbers.clone();
            return cloned;
        } catch (CloneNotSupportedException e)
        {
            throw new AssertionError();
        }
    }
}
Файл TextUtils:
public class TextUtils implements Cloneable
{
    private String text;

    public TextUtils(String text)
    {
        this.text = text;
    }

    public int countSentence()
    {
        int count = 0;
        for (int i = 0; i < text.length(); i++)
        {
            if (text.charAt(i) == '.' || text.charAt(i) == '!' || text.charAt(i) == '?')
            {
                count++;
            }
        }
        return count;
    }

    public String mostFreqntWord()
    {
        String[] words = text.toLowerCase().split("[\\s.,!?;:]+");
        int[] wordCounts = new int[words.length];
        int maxCount = 0;
        int maxIndex = 0;

        for (int i = 0; i < words.length; i++)
        {
            if (!words[i].isEmpty())
            {
                for (int j = 0; j < words.length; j++)
                {
                    if (words[i].equals(words[j]))
                    {
                        wordCounts[i]++;
                    }
                }
                if (wordCounts[i] > maxCount)
                {
                    maxCount = wordCounts[i];
                    maxIndex = i;
                }
            }
        }
        return words[maxIndex];
    }

    public int firstLetrVowel()
    {
        String[] words = text.toLowerCase().split("[\\s.,!?;:]+");
        int vowelCount = 0;

        for (String word : words)
        {
            if (!word.isEmpty() && isVowel(word.charAt(0)))
            {
                vowelCount++;
            }
        }
        return vowelCount;
    }

    public int firstLetrConsonant()
    {
        String[] words = text.toLowerCase().split("[\\s.,!?;:]+");
        int consonantCount = 0;

        for (String word : words)
        {
            if (!word.isEmpty() && !isVowel(word.charAt(0)))
            {
                consonantCount++;
            }
        }
        return consonantCount;
    }

    private boolean isVowel(char ch)
    {
        return "аеёиоуыэюя".indexOf(ch) != -1;
    }

    // Метод для получения текста
    public String getText()
    {
        return text;
    }

    public TextUtils clone()
    {
        try
        {
            return (TextUtils) super.clone();
        } catch (CloneNotSupportedException e)
        {
            throw new AssertionError();
        }
    }
}
