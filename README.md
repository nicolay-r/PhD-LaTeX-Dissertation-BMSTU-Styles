# Настройка стилей диссертационного шаблона под требования МГТУ им. Н.Э.Баумана

Все изменения приведенные ниже рассматриваются для шаблона версии `1.0.0` проекта 

Russian-Phd-Latex-Dissertation-Template

Ссылка на шаблон: [версия 1.0.0](https://github.com/AndreyAkinshin/Russian-Phd-LaTeX-Dissertation-Template/tree/v1.0.0)

## Содержание
* [Настройка библиографии](#настройка-библиографии)
* [Нумерация страниц автореферата](#нумерация-страниц-автореферата)
* [Настройка вертиальных отступов разделов](#настройка-вертиальных-отступов-разделов)
* [Работа со сносками в тексте](#работа-со-сносками-в-тексте)
* [Оформление приложения](#оформление-приложения)
  * [Отключение отображения приложений в ToC](#отключение-отображения-приложений-в-toc)  
* [Оформление таблиц и рисунков](#оформление-таблиц-и-рисунков)

### Настройка библиографии
* Скрытие секции Сведений об ответственности:
```latex
% 1. Необходимо закомментировать эту строку в biblio/biblatex.tex
\toggletrue{bbx:gostbibliography}%

% 2. Добавить movenames=false (чтобы при 4 и более авторов не 
% было отдельного выноса авторов в секцию об ответственности)
\usepackage[%
  % ...
  movenames=false,
  % ...
]{biblatex}[2016/09/17]
```

### Нумерация страниц автореферата
```latex
% Настройка колонтитулов в synopsis.tex
\makeevenhead{plain}{}{}{}
\makeoddhead{plain}{}{}{}
\makeevenfoot{plain}{\thepage}{}{}
\makeoddfoot{plain}{}{}{\thepage}
```

### Настройка вертиальных отступов разделов

```latex
% Подключаем пакет titlesec
\usepackage{titlesec}

% Редактируем отступы слева/сверху/снизу
% Используем ex -- высота `x` символа в тексте; 1.5ex ~ 1.5 отступ.
\titlespacing*{\section}{0pt}{2.0ex}{1.5ex}
\titlespacing*{\subsection}{0pt}{2.0ex}{1.5ex}
```

### Работа со сносками в тексте 

* Сноски не допустимы в текстах технической специальности МГТУ им. Н.Э.Баумана, 
поэтому подобную информацию размещаем прямо в тексте:

```latex
% Указание обычных сносок в тексте (вводим дополнительную команду \cfootnote)
\newcommand{\cfootnote}[1]{~({\small #1})} 

% настройка переноса URL-ссылок в тексте: 
% https://tex.stackexchange.com/questions/3033/forcing-linebreaks-in-url):
\usepackage{xurl}

% настройка ссылок на литературу в тексте
% Подключаем библиотеку для возможности вставки содержимого библиографии 
% в текст без явного указания и регистрации ссылок.
\usepackage{usebib}

\bibinput{eng_biblio}                   % Подключаем файл с литературой 
                                        % (поддерживается только английский язык!)
                                        % Его нужно создать отдельно и скопировать
                                        % туда записи, на которые хочется сослаться 
                                        % в автореферате

\newcommand{\cinplace}[2]{ {[%
    #1 и др.,                           % Инфморация об авторе
    \textit{\usebibentry{#2}{title}},   % Название работы 
    \usebibentry{#2}{year}]}}           % Год публикации работы
    
% Далее используем команду (пример)
\cinplace{Ivan}{author1999paper}
```

### Оформление приложения

В файле `Dissertation/appendix.tex`
```latex
% Переопределяем команду \chapter.
\newcommand{\mychapter}[2]{
    \setcounter{chapter}{#1}
    \setcounter{section}{0}
    \chapter*{#2}
    \addcontentsline{toc}{chapter}{#2}
}

% Задаем свою запись в ToC
\mychapter{1}{Приложения}

% Сбрасываем счетчик картинок. 
\setcounter{figure}{0}
\setcounter{table}{0}

% нумерация таблиц в приложении
\renewcommand\thetable{П.\arabic{table}} 

% нумерация рисунков в приложении
\renewcommand\thefigure{П.\arabic{figure}} 
```

#### Отключение отображения приложений в ToC

В файле `Dissertation/appendix.tex`
```latex
% Вводим команды для создания секции, которая не будет в ToC
\newcommand{\nocontentsline}[3]{}
\newcommand{\tocless}[2]{\bgroup\let\addcontentsline=\nocontentsline#1{#2}\egroup}

% Далее записываем в такой форме (наименование не будет попадать
% в раздел Table of Contents)
\tocless\section{Раздел приложения}
```

### Оформление таблиц и рисунков

```latex

% Смещаем метку в подписи в правый угол
\DeclareCaptionLabelFormat{righted-label}{\hspace*{\fill}#1 #2}

% Добавляем точку и перевод на новую строку после метки
\DeclareCaptionLabelSeparator{period-newline}{. \\}

% Указаны настройки, которые нужно обновить
\captionsetup[table]{
        % ...
        labelformat=righted-label,
        justification=centering,          % центровка
        labelsep=period-newline,          % разделитель
}

% Указаны настройки, которые нужно обновить
\captionsetup[figure]{
        % ...
        skip=4pt,                         % отбивка под подписью
        justification=centering,          % центровка
        labelsep=period-newline,          % разделитель
}
```
