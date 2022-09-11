import argparse
import re
import pickle
import random


class Text():
    def __init__(self, filename):
        self.train = filename

    def fit(self):
        # читаем из файла, парсим текст, генерим биграммы, lowercase - предобработка
        with open(self.train, 'r', encoding='utf-8') as file:
            text = file.read()

        cleaned_text = re.sub('[^а-яё]', ' ', text.lower())
        splited_text = re.split('\\s+', cleaned_text)

        word_dict = {}
        for i in range(0, len(splited_text) - 1):
            # исключаем повторения слов на стыках предложений
            if splited_text[i] != splited_text[i + 1]:
                if splited_text[i] not in word_dict:
                    word_dict[splited_text[i]] = [splited_text[i + 1]]
                else:
                    if splited_text[i + 1] not in word_dict[splited_text[i]]:
                        word_dict[splited_text[i]].append(splited_text[i + 1])

        return pickle.dumps(word_dict)

    def generate(self, length: int):
        bigrams = pickle.loads(self.fit())

        # в качестве первого слова будет идти то, после которого следует болше всего слов
        init_word = max(bigrams, key=lambda el: len(bigrams[el]))
        generated_text = [init_word]
        curr = init_word
        # генерируем новый текст длины length
        for i in range(length - 1):
            # выбираем случайно слово, следующее после текущего -> curr
            next = random.choice(bigrams[curr])
            generated_text.append(next)
            # обновляем текущее слово
            curr = next

        # создаем файл с новым текстом
        with open('generated.txt', 'w') as f:
            f.write(' '.join(generated_text))


def main():
    parser = argparse.ArgumentParser()
    parser.add_argument("--file", help="input filename")
    parser.add_argument("--length", help="length of output generated text")
    args = parser.parse_args()
    # print(args.file, args.length)
    filename = args.file
    length = int(args.length)
    # filename = 'input.txt'
    text_1 = Text(filename)
    text_1.generate(length)


if __name__ == '__main__':
    main()
