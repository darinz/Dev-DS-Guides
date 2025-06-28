# LaTeX Templates Collection

A comprehensive collection of LaTeX templates for various document types, from academic papers to professional reports and presentations.

## Table of Contents

1. [Academic Paper Template](#academic-paper-template)
2. [Thesis/Dissertation Template](#thesisdissertation-template)
3. [Technical Report Template](#technical-report-template)
4. [Beamer Presentation Template](#beamer-presentation-template)
5. [Letter Template](#letter-template)
6. [Resume/CV Template](#resumecv-template)
7. [Poster Template](#poster-template)
8. [Book Template](#book-template)
9. [Article with Bibliography](#article-with-bibliography)
10. [Algorithm Template](#algorithm-template)
11. [Code Listing Template](#code-listing-template)
12. [Custom Environments](#custom-environments)

## Academic Paper Template

### Basic Academic Paper

```latex
\documentclass[12pt,a4paper]{article}
\usepackage[utf8]{inputenc}
\usepackage[T1]{fontenc}
\usepackage{geometry}
\usepackage{amsmath}
\usepackage{amsfonts}
\usepackage{amssymb}
\usepackage{graphicx}
\usepackage{hyperref}
\usepackage{booktabs}
\usepackage{multirow}
\usepackage{subcaption}

% Page setup
\geometry{margin=2.5cm}

% Hyperref setup
\hypersetup{
    colorlinks=true,
    linkcolor=blue,
    filecolor=magenta,      
    urlcolor=cyan,
    citecolor=red
}

\title{Title of the Paper}
\author{Author Name\\
        Institution\\
        \texttt{email@institution.edu}}
\date{\today}

\begin{document}

\maketitle

\begin{abstract}
This is the abstract of the paper. It should provide a brief summary of the work, 
including the main objectives, methodology, results, and conclusions. The abstract 
should be concise but informative, typically between 150-250 words.
\end{abstract}

\section{Introduction}
\label{sec:introduction}

The introduction should provide the context and motivation for the work. 
It should clearly state the problem being addressed and the objectives of the study.

\subsection{Background}
\label{subsec:background}

Provide relevant background information and literature review.

\subsection{Objectives}
\label{subsec:objectives}

Clearly state the specific objectives of this work.

\section{Methodology}
\label{sec:methodology}

Describe the methods and approaches used in the study.

\subsection{Data Collection}
\label{subsec:data}

Explain how data was collected and processed.

\subsection{Analysis Methods}
\label{subsec:analysis}

Describe the analytical methods and tools used.

\section{Results}
\label{sec:results}

Present the main findings of the study.

\subsection{Statistical Analysis}
\label{subsec:stats}

Present statistical results and their interpretation.

\section{Discussion}
\label{sec:discussion}

Discuss the implications of the results and their significance.

\section{Conclusion}
\label{sec:conclusion}

Summarize the main findings and their implications.

\section{Future Work}
\label{sec:future}

Suggest directions for future research.

\bibliographystyle{plain}
\bibliography{references}

\end{document}
```

### IEEE Conference Paper

```latex
\documentclass[conference]{IEEEtran}
\usepackage[utf8]{inputenc}
\usepackage{amsmath}
\usepackage{amsfonts}
\usepackage{amssymb}
\usepackage{graphicx}
\usepackage{cite}
\usepackage{url}

\title{Title of the IEEE Conference Paper}

\author{\IEEEauthorblockN{Author Name}
\IEEEauthorblockA{Department\\
Institution\\
City, Country\\
Email: author@institution.edu}}

\begin{document}

\maketitle

\begin{abstract}
This is the abstract for an IEEE conference paper. It should be concise and 
informative, typically around 150 words.
\end{abstract}

\IEEEpeerreviewmaketitle

\section{Introduction}
\label{sec:introduction}

The introduction follows IEEE format guidelines.

\section{Related Work}
\label{sec:related}

Review of relevant literature and previous work.

\section{Methodology}
\label{sec:methodology}

Description of the proposed approach.

\section{Experimental Results}
\label{sec:results}

Presentation and analysis of experimental results.

\section{Conclusion}
\label{sec:conclusion}

Summary of contributions and future work.

\bibliographystyle{IEEEtran}
\bibliography{references}

\end{document}
```

## Thesis/Dissertation Template

```latex
\documentclass[12pt,oneside]{report}
\usepackage[utf8]{inputenc}
\usepackage[T1]{fontenc}
\usepackage{geometry}
\usepackage{amsmath}
\usepackage{amsfonts}
\usepackage{amssymb}
\usepackage{graphicx}
\usepackage{hyperref}
\usepackage{booktabs}
\usepackage{multirow}
\usepackage{subcaption}
\usepackage{listings}
\usepackage{color}
\usepackage{fancyhdr}
\usepackage{titlesec}

% Page setup
\geometry{margin=2.5cm}

% Header and footer
\pagestyle{fancy}
\fancyhf{}
\fancyhead[L]{\leftmark}
\fancyhead[R]{\thepage}
\renewcommand{\headrulewidth}{0.4pt}

% Chapter title formatting
\titleformat{\chapter}[display]
{\normalfont\huge\bfseries}{\chaptertitlename\ \thechapter}{20pt}{\Huge}

% Code listing setup
\lstset{
    basicstyle=\ttfamily\small,
    breaklines=true,
    frame=single,
    numbers=left,
    numberstyle=\tiny,
    keywordstyle=\color{blue},
    commentstyle=\color{green!60!black},
    stringstyle=\color{red}
}

\title{Thesis Title: A Comprehensive Study}
\author{Student Name}
\date{\today}

\begin{document}

% Front matter
\pagenumbering{roman}

% Title page
\begin{titlepage}
\begin{center}
\vspace*{2cm}
{\Huge \textbf{Thesis Title: A Comprehensive Study}}

\vspace{1cm}
{\Large A thesis submitted in partial fulfillment of the requirements\\
for the degree of Master of Science}

\vspace{1cm}
{\large in Computer Science}

\vspace{1cm}
{\large by}

\vspace{0.5cm}
{\Large \textbf{Student Name}}

\vspace{1cm}
{\large Under the supervision of\\
Prof. Supervisor Name}

\vspace{1cm}
{\large Department of Computer Science\\
University Name\\
City, Country}

\vspace{1cm}
{\large \today}
\end{center}
\end{titlepage}

% Abstract
\chapter*{Abstract}
\addcontentsline{toc}{chapter}{Abstract}

This is the abstract of the thesis. It should provide a comprehensive overview 
of the research, including the problem statement, methodology, main findings, 
and contributions. The abstract should be between 300-500 words.

\chapter*{Acknowledgments}
\addcontentsline{toc}{chapter}{Acknowledgments}

I would like to express my gratitude to my supervisor, Prof. Supervisor Name, 
for their guidance and support throughout this research project.

% Table of contents
\tableofcontents
\listoffigures
\listoftables

% Main matter
\pagenumbering{arabic}

\chapter{Introduction}
\label{chap:introduction}

\section{Background and Motivation}
\label{sec:background}

Provide the context and motivation for the research.

\section{Problem Statement}
\label{sec:problem}

Clearly define the research problem and objectives.

\section{Research Contributions}
\label{sec:contributions}

Outline the main contributions of this thesis.

\section{Thesis Organization}
\label{sec:organization}

Describe the structure and organization of the thesis.

\chapter{Literature Review}
\label{chap:literature}

\section{Related Work}
\label{sec:related}

Comprehensive review of relevant literature.

\section{Research Gaps}
\label{sec:gaps}

Identify gaps in current research that this work addresses.

\chapter{Methodology}
\label{chap:methodology}

\section{Research Design}
\label{sec:design}

Describe the overall research design and approach.

\section{Data Collection}
\label{sec:data}

Explain the data collection methods and procedures.

\section{Analysis Methods}
\label{sec:analysis}

Detail the analytical methods and tools used.

\chapter{Results and Analysis}
\label{chap:results}

\section{Experimental Setup}
\label{sec:setup}

Describe the experimental setup and parameters.

\section{Results}
\label{sec:results}

Present and analyze the experimental results.

\section{Discussion}
\label{sec:discussion}

Discuss the implications and significance of the results.

\chapter{Conclusion and Future Work}
\label{chap:conclusion}

\section{Summary of Contributions}
\label{sec:summary}

Summarize the main contributions of this research.

\section{Limitations}
\label{sec:limitations}

Discuss the limitations of the current work.

\section{Future Work}
\label{sec:future}

Suggest directions for future research.

% Back matter
\appendix

\chapter{Appendix A: Additional Data}
\label{app:data}

Additional data and supplementary materials.

\chapter{Appendix B: Mathematical Proofs}
\label{app:proofs}

Detailed mathematical proofs and derivations.

% Bibliography
\bibliographystyle{plain}
\bibliography{references}

\end{document}
```

## Technical Report Template

```latex
\documentclass[12pt,a4paper]{report}
\usepackage[utf8]{inputenc}
\usepackage[T1]{fontenc}
\usepackage{geometry}
\usepackage{amsmath}
\usepackage{amsfonts}
\usepackage{amssymb}
\usepackage{graphicx}
\usepackage{hyperref}
\usepackage{booktabs}
\usepackage{multirow}
\usepackage{subcaption}
\usepackage{listings}
\usepackage{color}
\usepackage{fancyhdr}
\usepackage{titlesec}

% Page setup
\geometry{margin=2.5cm}

% Header and footer
\pagestyle{fancy}
\fancyhf{}
\fancyhead[L]{\leftmark}
\fancyhead[R]{\thepage}
\renewcommand{\headrulewidth}{0.4pt}

% Code listing setup
\lstset{
    basicstyle=\ttfamily\small,
    breaklines=true,
    frame=single,
    numbers=left,
    numberstyle=\tiny,
    keywordstyle=\color{blue},
    commentstyle=\color{green!60!black},
    stringstyle=\color{red}
}

\title{Technical Report Title}
\author{Author Name\\
        Organization\\
        \texttt{email@organization.com}}
\date{\today}

\begin{document}

\maketitle

\begin{abstract}
This technical report presents the findings of [project/study name]. 
The report includes methodology, results, analysis, and recommendations.
\end{abstract}

\tableofcontents
\listoffigures
\listoftables

\chapter{Executive Summary}
\label{chap:executive}

Brief overview of the report contents and key findings.

\chapter{Introduction}
\label{chap:introduction}

\section{Purpose}
\label{sec:purpose}

State the purpose and scope of this technical report.

\section{Background}
\label{sec:background}

Provide necessary background information.

\section{Objectives}
\label{sec:objectives}

List the specific objectives of this report.

\chapter{Methodology}
\label{chap:methodology}

\section{Approach}
\label{sec:approach}

Describe the overall approach and methodology.

\section{Data Sources}
\label{sec:data}

Explain the data sources and collection methods.

\section{Analysis Methods}
\label{sec:analysis}

Detail the analytical methods used.

\chapter{Results}
\label{chap:results}

\section{Findings}
\label{sec:findings}

Present the main findings and results.

\section{Analysis}
\label{sec:analysis}

Analyze and interpret the results.

\chapter{Discussion}
\label{chap:discussion}

\section{Implications}
\label{sec:implications}

Discuss the implications of the findings.

\section{Recommendations}
\label{sec:recommendations}

Provide specific recommendations based on the findings.

\chapter{Conclusion}
\label{chap:conclusion}

Summarize the key points and conclusions.

\appendix

\chapter{Appendix A: Technical Details}
\label{app:technical}

Additional technical details and specifications.

\chapter{Appendix B: Data Tables}
\label{app:data}

Detailed data tables and supplementary information.

\bibliographystyle{plain}
\bibliography{references}

\end{document}
```

## Beamer Presentation Template

```latex
\documentclass[aspectratio=169]{beamer}
\usepackage[utf8]{inputenc}
\usepackage{amsmath}
\usepackage{amsfonts}
\usepackage{amssymb}
\usepackage{graphicx}
\usepackage{hyperref}
\usepackage{booktabs}
\usepackage{multirow}

% Theme setup
\usetheme{Madrid}
\usecolortheme{default}
\setbeamertemplate{navigation symbols}{}

% Custom colors
\definecolor{myblue}{RGB}{0,114,178}
\definecolor{myorange}{RGB}{230,159,0}
\setbeamercolor{title}{fg=myblue}
\setbeamercolor{frametitle}{fg=myblue}

% Title page
\title{Presentation Title}
\subtitle{Subtitle or Conference Name}
\author{Author Name\\
        Institution\\
        \texttt{email@institution.edu}}
\date{\today}

\begin{document}

% Title frame
\begin{frame}
\titlepage
\end{frame}

% Outline frame
\begin{frame}{Outline}
\tableofcontents
\end{frame}

\section{Introduction}
\label{sec:introduction}

\begin{frame}{Introduction}
\frametitle{Introduction}

\begin{itemize}
\item<1-> First point
\item<2-> Second point
\item<3-> Third point
\end{itemize}

\begin{block}{Key Message}
This is an important message highlighted in a block.
\end{block}

\end{frame}

\begin{frame}{Background}
\frametitle{Background}

\begin{columns}
\begin{column}{0.5\textwidth}
\begin{itemize}
\item Background point 1
\item Background point 2
\item Background point 3
\end{itemize}
\end{column}
\begin{column}{0.5\textwidth}
\includegraphics[width=\textwidth]{figure.png}
\end{column}
\end{columns}

\end{frame}

\section{Methodology}
\label{sec:methodology}

\begin{frame}{Methodology}
\frametitle{Methodology}

\begin{enumerate}
\item<1-> Step 1: Data collection
\item<2-> Step 2: Preprocessing
\item<3-> Step 3: Analysis
\item<4-> Step 4: Evaluation
\end{enumerate}

\end{frame}

\begin{frame}{Experimental Setup}
\frametitle{Experimental Setup}

\begin{table}
\centering
\begin{tabular}{lc}
\toprule
Parameter & Value \\
\midrule
Dataset size & 10,000 samples \\
Training time & 2 hours \\
Accuracy & 95\% \\
\bottomrule
\end{tabular}
\caption{Experimental parameters}
\end{table}

\end{frame}

\section{Results}
\label{sec:results}

\begin{frame}{Results}
\frametitle{Results}

\begin{figure}
\centering
\includegraphics[width=0.8\textwidth]{results.png}
\caption{Performance comparison}
\end{figure}

\end{frame}

\begin{frame}{Key Findings}
\frametitle{Key Findings}

\begin{block}{Finding 1}
Description of the first key finding.
\end{block}

\begin{block}{Finding 2}
Description of the second key finding.
\end{block}

\begin{block}{Finding 3}
Description of the third key finding.
\end{block}

\end{frame}

\section{Conclusion}
\label{sec:conclusion}

\begin{frame}{Conclusion}
\frametitle{Conclusion}

\begin{itemize}
\item Summary of contributions
\item Future work directions
\item Questions and discussion
\end{itemize}

\begin{center}
\Large Thank you for your attention!
\end{center}

\end{frame}

% Backup slides
\appendix

\begin{frame}[allowframebreaks]{References}
\frametitle{References}

\begin{thebibliography}{10}
\bibitem{ref1} Author, A. (2023). Title of the paper. Journal Name, 1(1), 1-10.
\bibitem{ref2} Author, B. (2023). Another paper title. Conference Name, 1-5.
\end{thebibliography}

\end{frame}

\end{document}
```

## Letter Template

```latex
\documentclass[12pt]{letter}
\usepackage[utf8]{inputenc}
\usepackage{geometry}
\usepackage{hyperref}

% Page setup
\geometry{margin=2.5cm}

% Letter information
\address{Your Name\\
Your Address\\
City, State ZIP\\
Phone: (555) 123-4567\\
Email: your.email@example.com}

\signature{Your Name}

\begin{document}

\begin{letter}{Recipient Name\\
              Recipient Title\\
              Organization Name\\
              Organization Address\\
              City, State ZIP}

\opening{Dear Mr./Ms. Recipient Name,}

This is the opening paragraph of your letter. It should introduce the purpose 
of the letter and establish the context for the reader.

The body of the letter should contain the main content. You can have multiple 
paragraphs to organize your thoughts and present your information clearly. 
Each paragraph should focus on a specific point or aspect of your message.

In the closing paragraph, you should summarize your main points, restate your 
purpose if necessary, and indicate what you expect or hope will happen next.

\closing{Sincerely,}

\ps{P.S. This is a postscript if you need to add something after your signature.}

\end{letter}

\end{document}
```

## Resume/CV Template

```latex
\documentclass[11pt,a4paper]{article}
\usepackage[utf8]{inputenc}
\usepackage{geometry}
\usepackage{hyperref}
\usepackage{enumitem}
\usepackage{titlesec}

% Page setup
\geometry{margin=1in}

% Custom commands
\newcommand{\sectiontitle}[1]{\section*{#1}\vspace{-0.5em}\hrule\vspace{0.5em}}

% Remove page numbers
\pagestyle{empty}

\begin{document}

% Header
\begin{center}
{\Huge \textbf{Your Name}}\\[0.5em]
{\large Your Title}\\[0.5em]
\href{mailto:your.email@example.com}{your.email@example.com} $|$ 
\href{tel:+15551234567}{(555) 123-4567} $|$ 
\href{https://linkedin.com/in/yourprofile}{LinkedIn} $|$ 
\href{https://github.com/yourusername}{GitHub}
\end{center}

\vspace{1em}

% Summary
\sectiontitle{Professional Summary}
Experienced professional with expertise in [your field]. Demonstrated track record 
of [key achievements]. Skilled in [key skills] with a passion for [your interests].

% Experience
\sectiontitle{Professional Experience}

\textbf{Job Title} \hfill \textbf{Company Name} \\
\textit{Location} \hfill \textit{Date Range} \\
\begin{itemize}[leftmargin=*]
\item Key achievement or responsibility
\item Another key achievement or responsibility
\item Third key achievement or responsibility
\end{itemize}

\textbf{Previous Job Title} \hfill \textbf{Previous Company} \\
\textit{Location} \hfill \textit{Date Range} \\
\begin{itemize}[leftmargin=*]
\item Key achievement or responsibility
\item Another key achievement or responsibility
\end{itemize}

% Education
\sectiontitle{Education}

\textbf{Degree Name} \hfill \textbf{Institution Name} \\
\textit{Field of Study} \hfill \textit{Graduation Year} \\
GPA: X.XX/4.00

% Skills
\sectiontitle{Technical Skills}

\textbf{Programming Languages:} Language 1, Language 2, Language 3 \\
\textbf{Tools \& Technologies:} Tool 1, Tool 2, Tool 3 \\
\textbf{Frameworks \& Libraries:} Framework 1, Framework 2 \\
\textbf{Databases:} Database 1, Database 2 \\
\textbf{Other Skills:} Skill 1, Skill 2, Skill 3

% Projects
\sectiontitle{Projects}

\textbf{Project Name} \hfill \textbf{Technologies Used} \\
\textit{Description of the project and your role.} \\
\begin{itemize}[leftmargin=*]
\item Key feature or contribution
\item Another key feature or contribution
\end{itemize}

% Certifications
\sectiontitle{Certifications}

\textbf{Certification Name} \hfill \textbf{Issuing Organization} \\
\textit{Date Obtained}

% Awards
\sectiontitle{Awards \& Honors}

\textbf{Award Name} \hfill \textbf{Year} \\
\textit{Description of the award and criteria}

\end{document}
```

## Poster Template

```latex
\documentclass[portrait,a0b]{a0poster}
\usepackage[utf8]{inputenc}
\usepackage{amsmath}
\usepackage{amsfonts}
\usepackage{amssymb}
\usepackage{graphicx}
\usepackage{multicol}
\usepackage{geometry}

% Custom colors
\definecolor{myblue}{RGB}{0,114,178}
\definecolor{myorange}{RGB}{230,159,0}

% Title formatting
\title{\Huge \textbf{Poster Title}\\[0.5em]
       \Large Subtitle or Conference Name}
\author{\Large Author Name$^{1}$, Co-author Name$^{2}$}
\institute{\large $^{1}$Institution 1, $^{2}$Institution 2\\
           \texttt{email@institution.edu}}

\begin{document}

% Title section
\begin{center}
\maketitle
\end{center}

\vspace{1cm}

\begin{multicols}{3}

\section*{Introduction}
This is the introduction section of your poster. It should provide the context 
and motivation for your work. Keep it concise but informative.

\section*{Objectives}
\begin{itemize}
\item Objective 1
\item Objective 2
\item Objective 3
\end{itemize}

\section*{Methodology}
Describe your methodology here. You can include:
\begin{itemize}
\item Data collection methods
\item Analysis techniques
\item Experimental setup
\end{itemize}

\section*{Results}
Present your main results here. You can include:
\begin{itemize}
\item Key findings
\item Statistical results
\item Performance metrics
\end{itemize}

\section*{Conclusions}
Summarize your main conclusions and their implications.

\section*{Future Work}
Outline directions for future research.

\section*{References}
\begin{enumerate}
\item Reference 1
\item Reference 2
\item Reference 3
\end{enumerate}

\end{multicols}

\end{document}
```

## Book Template

```latex
\documentclass[12pt,oneside]{book}
\usepackage[utf8]{inputenc}
\usepackage[T1]{fontenc}
\usepackage{geometry}
\usepackage{amsmath}
\usepackage{amsfonts}
\usepackage{amssymb}
\usepackage{graphicx}
\usepackage{hyperref}
\usepackage{fancyhdr}
\usepackage{titlesec}

% Page setup
\geometry{margin=2.5cm}

% Header and footer
\pagestyle{fancy}
\fancyhf{}
\fancyhead[LE,RO]{\thepage}
\fancyhead[RE]{\leftmark}
\fancyhead[LO]{\rightmark}
\renewcommand{\headrulewidth}{0.4pt}

% Chapter title formatting
\titleformat{\chapter}[display]
{\normalfont\huge\bfseries}{\chaptertitlename\ \thechapter}{20pt}{\Huge}

\title{Book Title}
\author{Author Name}
\date{\today}

\begin{document}

% Front matter
\frontmatter

% Title page
\maketitle

% Copyright page
\thispagestyle{empty}
\vspace*{\fill}
\begin{center}
Copyright \copyright\ 2023 by Author Name\\
All rights reserved.
\end{center}
\vspace*{\fill}
\newpage

% Table of contents
\tableofcontents
\listoffigures
\listoftables

% Main matter
\mainmatter

\chapter{Introduction}
\label{chap:introduction}

\section{Background}
\label{sec:background}

This is the background section of the book.

\section{Objectives}
\label{sec:objectives}

The objectives of this book are outlined here.

\chapter{Chapter Title}
\label{chap:chapter}

\section{Section Title}
\label{sec:section}

Content of the section goes here.

\subsection{Subsection Title}
\label{subsec:subsection}

Content of the subsection goes here.

\chapter{Another Chapter}
\label{chap:another}

More content for another chapter.

% Back matter
\backmatter

\chapter*{Bibliography}
\addcontentsline{toc}{chapter}{Bibliography}

\bibliographystyle{plain}
\bibliography{references}

\chapter*{Index}
\addcontentsline{toc}{chapter}{Index}

% Add index entries here

\end{document}
```

## Article with Bibliography

```latex
\documentclass[12pt,a4paper]{article}
\usepackage[utf8]{inputenc}
\usepackage[T1]{fontenc}
\usepackage{geometry}
\usepackage{amsmath}
\usepackage{amsfonts}
\usepackage{amssymb}
\usepackage{graphicx}
\usepackage{hyperref}
\usepackage{natbib}

% Page setup
\geometry{margin=2.5cm}

% Bibliography style
\bibliographystyle{plainnat}

\title{Article Title}
\author{Author Name\\
        Institution\\
        \texttt{email@institution.edu}}
\date{\today}

\begin{document}

\maketitle

\begin{abstract}
This is the abstract of the article. It should provide a brief summary of the work.
\end{abstract}

\section{Introduction}
\label{sec:introduction}

The introduction should provide context and motivation \citep{reference1}. 
Previous work has shown that \citep{reference2}.

\section{Related Work}
\label{sec:related}

This section reviews related work in the field \citep{reference3}.

\section{Methodology}
\label{sec:methodology}

Our approach builds upon the work of \citet{reference4}.

\section{Results}
\label{sec:results}

The results demonstrate significant improvements over previous methods 
\citep[see][p.~45]{reference5}.

\section{Conclusion}
\label{sec:conclusion}

We have presented a novel approach that addresses the limitations 
identified by \citet{reference6}.

\bibliography{references}

\end{document}
```

## Algorithm Template

```latex
\documentclass[12pt,a4paper]{article}
\usepackage[utf8]{inputenc}
\usepackage{amsmath}
\usepackage{algorithm}
\usepackage{algorithmic}
\usepackage{graphicx}

\title{Algorithm Description}
\author{Author Name}
\date{\today}

\begin{document}

\maketitle

\section{Algorithm Description}

\begin{algorithm}
\caption{Algorithm Name}
\label{alg:algorithm}
\begin{algorithmic}[1]
\REQUIRE Input parameters
\ENSURE Output results
\STATE Initialize variables
\FOR{each item in dataset}
    \STATE Process item
    \IF{condition is met}
        \STATE Perform action A
    \ELSE
        \STATE Perform action B
    \ENDIF
\ENDFOR
\STATE Return results
\end{algorithmic}
\end{algorithm}

\section{Complexity Analysis}

The time complexity of Algorithm~\ref{alg:algorithm} is $O(n^2)$ and 
the space complexity is $O(n)$.

\end{document}
```

## Code Listing Template

```latex
\documentclass[12pt,a4paper]{article}
\usepackage[utf8]{inputenc}
\usepackage{listings}
\usepackage{color}
\usepackage{graphicx}

% Code listing setup
\lstset{
    basicstyle=\ttfamily\small,
    breaklines=true,
    frame=single,
    numbers=left,
    numberstyle=\tiny,
    keywordstyle=\color{blue},
    commentstyle=\color{green!60!black},
    stringstyle=\color{red},
    showstringspaces=false,
    tabsize=4
}

\title{Code Documentation}
\author{Author Name}
\date{\today}

\begin{document}

\maketitle

\section{Python Code Example}

\begin{lstlisting}[language=Python, caption=Python function example]
def fibonacci(n):
    """
    Calculate the nth Fibonacci number.
    
    Args:
        n (int): The position in the Fibonacci sequence
        
    Returns:
        int: The nth Fibonacci number
    """
    if n <= 1:
        return n
    else:
        return fibonacci(n-1) + fibonacci(n-2)

# Example usage
result = fibonacci(10)
print(f"The 10th Fibonacci number is: {result}")
\end{lstlisting}

\section{JavaScript Code Example}

\begin{lstlisting}[language=JavaScript, caption=JavaScript function example]
function factorial(n) {
    // Calculate factorial of n
    if (n <= 1) {
        return 1;
    } else {
        return n * factorial(n - 1);
    }
}

// Example usage
const result = factorial(5);
console.log(`5! = ${result}`);
\end{lstlisting}

\section{SQL Query Example}

\begin{lstlisting}[language=SQL, caption=SQL query example]
SELECT 
    u.name,
    COUNT(o.id) as order_count,
    SUM(o.total) as total_spent
FROM users u
LEFT JOIN orders o ON u.id = o.user_id
WHERE u.created_at >= '2023-01-01'
GROUP BY u.id, u.name
HAVING COUNT(o.id) > 0
ORDER BY total_spent DESC
LIMIT 10;
\end{lstlisting}

\end{document}
```

## Custom Environments

```latex
\documentclass[12pt,a4paper]{article}
\usepackage[utf8]{inputenc}
\usepackage{amsmath}
\usepackage{amsthm}
\usepackage{graphicx}

% Custom theorem environments
\newtheorem{theorem}{Theorem}[section]
\newtheorem{lemma}[theorem]{Lemma}
\newtheorem{proposition}[theorem]{Proposition}
\newtheorem{corollary}[theorem]{Corollary}
\newtheorem{definition}[theorem]{Definition}
\newtheorem{example}[theorem]{Example}
\newtheorem{remark}[theorem]{Remark}

% Custom proof environment
\renewcommand{\qedsymbol}{$\blacksquare$}

\title{Custom Environments Example}
\author{Author Name}
\date{\today}

\begin{document}

\maketitle

\section{Mathematical Content}

\begin{definition}
A function $f: \mathbb{R} \to \mathbb{R}$ is said to be continuous at a point 
$x_0$ if for every $\epsilon > 0$, there exists a $\delta > 0$ such that 
$|f(x) - f(x_0)| < \epsilon$ whenever $|x - x_0| < \delta$.
\end{definition}

\begin{theorem}
If $f$ is continuous on a closed interval $[a,b]$, then $f$ attains its 
maximum and minimum values on $[a,b]$.
\end{theorem}

\begin{proof}
The proof follows from the Extreme Value Theorem. Since $f$ is continuous 
on the closed interval $[a,b]$, the image of $[a,b]$ under $f$ is also 
a closed interval, which must have a maximum and minimum.
\end{proof}

\begin{example}
Consider the function $f(x) = x^2$ on the interval $[-1, 2]$. This function 
is continuous and attains its minimum value of $0$ at $x = 0$ and its 
maximum value of $4$ at $x = 2$.
\end{example}

\begin{remark}
This example demonstrates the importance of the closed interval requirement 
in the theorem statement.
\end{remark}

\end{document}
```

These templates provide a solid foundation for various types of LaTeX documents. You can customize them further based on your specific needs and requirements.
