% Ord Manual

# ord

## Intro

Recenser les disponibilités. 

Recenser les questions d'intérêts. 

Réunir les personnes autour des questions d'intérêt selon leur disponibilités.

Organiser un rendez-vous hebdomadaire avec des thèmes décidés par vote.

Organiser le temps de parole des intervenants par vote.


## Applets

### Concitoyen

<!---
FELLOW-CITIZEN:
-->
#### Actions

-   Participations
-   Thématiques
-   Commenter le concitoyen

#### Données

``` {.clojure}
{
   :id ObjectId
   :name String
   :token String
}
```

#### Présentation

#### Diagramme


### Questions

<!---
QUESTIONS:
-->
#### Actions

-   Proposer une question
-   Voter pour une question
-   Marquer l'intérêt pour une question
-   Donner mon temps de parole à un concitoyen sur cette question

#### Données

``` {.clojure}
{
   :id ObjectId
   :question String
   :author Citizen
   :date Date
   :upvotes Set<Hash<Citizen, Time>>
}
```

#### Présentation

\begin{tikzpicture}
  \draw[thick,solid] (0, 0) rectangle (5,-7);

  \draw[solid] (1,-1) rectangle (4,-2) node[pos=.5] {QuestionForm};
  \draw[solid] (1,-3) rectangle (4,-4) node[pos=.5] {Time Repartition};
  \draw[solid] (1,-5) rectangle (4,-6) node[pos=.5] {Participants};

  \draw[solid]  (6,0) rectangle (10,-2) node[pos=.5] {Question};
  \draw[dashed] (10,0) rectangle (12,-2) node[pos=.5] {up};

  \draw[solid] (9,-5) circle (2);

  \draw[solid] (9,-5) -- (9,-3);
  \draw[solid] (9,-5) -- (11,-5);
  \draw[solid] (9,-5) -- (9-2*0.7,-5-2*0.7);
  \draw[solid] (9,-5) -- (9-2*0.7,-5+2*0.7);
\end{tikzpicture}

#### Diagramme

### Commentaires

<!---
COMMENTS:
-->
#### Actions

-   Commenter une question
-   Commenter un commentaire

#### Données

``` {.clojure}
{
   :id ObjectId
   :comment String
   :on RefObjectId
   :author Citizen
   :date Date
   :upvotes Set<Citizen>
}
```

#### Présentation

\begin{tikzpicture}
  \draw[solid] (0,0) rectangle (5,-6);
\end{tikzpicture}

#### Diagramme

### Disponibilités

-   Indiquer les disponibilités
-   Indiquer les indisponibilités

### Diffusion

#### Actions

-   Programmer une diffusion
-   Inviter les participants
-   Promouvoir la diffusion

#### Données

``` {.clojure}
{
   :id ObjectId
   :date Number
   :questions Set<Question>
   :participants Set<Hash<Citizen, Time>>
}
```

#### Présentation


#### Diagramme

## Architecture

\begin{tikzpicture}
  \tikzstyle{dashedWhite}=[thick,dashed]
  \tikzstyle{solidGray}=[thick,solid,fill=gray!10]

  \draw[dashedWhite] (-6, 0) rectangle (-1,-1) node[pos=.5] {Client};
  \draw[solidGray]   (-6,-1) rectangle (-1,-6);
  \node[draw,fill=white] (CL)  at (-5,-3) {UI};
  \node[draw,fill=white] (CS)  at (-3,-2) {Store};
  \node[draw,fill=white] (CD)  at (-3,-3) {Dispatcher};
  \node[draw,fill=white] (CM1) at (-3,-4)   {Mutation};
  \node[draw,fill=white] (CM2) at (-3,-4.5) {Mutation};
  \node[draw,fill=white] (CR1) at (-3,-5)   {Read};

  \draw[dashedWhite] ( 6, 0) rectangle ( 1,-1) node[pos=.5] {Server};
  \draw[solidGray]   ( 6,-1) rectangle ( 1,-6);
  \node[draw,fill=white] (SL)  at ( 5,-3) {DB};
  \node[draw,fill=white] (SS)  at ( 3,-2) {Store};
  \node[draw,fill=white] (SD)  at ( 3,-3) {Dispatcher};
  \node[draw,fill=white] (SR1) at ( 3,-4)   {Read};
  \node[draw,fill=white] (SM1) at ( 3,-4.5) {Mutation};
  \node[draw,fill=white] (SM2) at ( 3,-5)   {Mutation};


  \draw[<->,>=latex] (CD) -- (SD) node[midway,above] {Isomorphic}
                                  node[midway,below] {Replication};

  \draw[->,>=latex]  (CM1) -- (CD);
  \draw[<->,>=latex] (CD)  -- (CS);
  \draw[->,>=latex]  (CD)  -- (CL);
  \draw[->,>=latex]  (CL) to[bend right] (CR1.west);

  \draw[->,>=latex]  (SR1) -- (SD);
  \draw[<->,>=latex] (SD)  -- (SS);
  \draw[->,>=latex]  (SD)  -- (SL);
  \draw[->,>=latex]  (SL) to[bend left] (SM2.east);

\end{tikzpicture}

-   Question
-   Comment
-   QuestionsList
-   CommentsList
-   Upvotes
