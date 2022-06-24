# compiler
Assignment – 1
Objective: Design a LEX Code to count the number of lines, space, tab-meta character and rest of characters in a given Input pattern.
Code:
%{
#include<stdio.h>
int lc=0, sc=0, tc=0, chc=0; %}
%%
\n lc++; [ ] sc++; \t tc++; . chc++; %%
int yywrap(void) {}
int main()
{
yylex();
printf("\nTotal Lines = %d\n",lc); printf("\nTotal spaces = %d\n",sc); printf("\nTotal Tabs = %d\n",tc); printf("\nTotal Characters = %d\n",chc); return 0;

}
--------------------
Assignment – 2
Objective: Design a LEX Code to identify and print valid Identifier of C/C++ in given Input pattern.
Code:
%{ #include<stdio.h>int c=0;%} %%
[a-zA-Z_][a-zA-Z0-9]*
.;
%%
int main(){
yylex();
printf("\nTotal number of valid Identifier = %d \n",c); }

-------------------
3)
%{
#include<stdio.h>
%}
%%
[0-9]+"."[0-9] {printf("\nDecimal Number\n");} [0-9]+ {printf("\nInteger Number\n");}
%%
int yywrap(void){}
int main()

{
yylex();return 0;}

------------------
4)
 Code: 
%{
int n=0; 
%}

%%
"while"|"if"|"else" {n++; printf("\t Keywords : %s",yytext);}
"int"|"float" {n++; printf("\t Keywords : %s",yytext);}
[a-zA-Z_][a-zA-Z0-9_]* {n++; printf("\t Identifier : %s",yytext);} "<="|"=="|"="|"++"|"+"|"-"|"*"|"/" {n++; printf("\t Operator : %s",yytext);} "("|")"|"{"|"}"|","|";" {n++; printf("\t Seperator : %s",yytext);}
[0-9]*"."[0-9]+ {n++; printf("\t Float %s",yytext);}
[0-9]+ {n++; printf("\t Integer : %s",yytext);}
,;
%%
int main() {
yylex();
printf("\nTotal number of tokens are %d",n);
}
------------------
5)

%{
int n,w,c; %}
%%
\n n++; [^ \n\t]+
. c++; %%
{w++; c=c+yyleng;}
int main()
{
extern FILE *yyin;
yyin = fopen("file","r");
yylex();
printf("line = %d\nword = %d\ncharacter = %d\n",n,w,c); 
}
-----------------

6)

%{
%}

%%
[\t\n]+ fprintf(yyout," ");
. fprintf(yyout,"%s",yytext); %%
int main()
{
extern FILE *yyin, *yyout;
yyin = fopen("file","r"); //r for read.
yyout = fopen("output","w"); //w for write. yylex();
}
-----------------

7)

%{ #include<stdio.h> %}
%%
\/\/(.*) {}; \/\*(.*\n)*.*\*\/ {}; %%
int yywrap() {
return 1;
}

int main()
{
yyin = fopen("input8.c","r"); yyout = fopen("output8.txt","w"); yylex();
return 0; 
}
----------------

8)

%{ #include<stdio.h> %}
%%
\<[^>]*\> fprintf(yyout,"%s\n",yytext); .|\n;
%%
int yywrap()

{
return 1; }
int main()
{
yyin = fopen("input7.html","r"); yyout = fopen("output7.txt","w"); yylex();
return 0;
}

---------------------

9)

%{ %}
 %s A B
%%
<INITIAL>a BEGIN INITIAL;
<INITIAL>b BEGIN A;
<INITIAL>[^0|\n] BEGIN B;
<INITIAL>\n BEGIN INITIAL; printf("Accepted\n"); <A>a BEGIN A;
<A>b BEGIN INITIAL;
<A>[^0|\n] BEGIN B;
<A>\n BEGIN INITIAL; printf("Not Accepted\n"); <B>b BEGIN B;
<B>a BEGIN B;
 <B>[^0|\n] BEGIN B;
<B>\n {BEGIN INITIAL; printf("INVALID\n");} %%
voidmain() {
yylex();
}

---------------------

10)

%{
%}
%s A B C D E F G DEAD
%%
<INITIAL>b BEGIN INITIAL;
<INITIAL>a BEGIN A;
<INITIAL>[^ab\n] BEGIN DEAD;
<INITIAL>\n BEGIN INITIAL; {printf("Not Accepted\n");}
<A>b BEGIN F;
<A>a BEGIN B;
<A>[^ab\n] BEGIN DEAD;
<A>\n BEGIN INITIAL; {printf("Not Accepted\n");}
<B>b BEGIN D;
<B>a BEGIN C;
<B>[^ab\n] BEGIN DEAD;
<B>\n BEGIN INITIAL; {printf("Not Accepted\n");}
<C>b BEGIN D;
<C>a BEGIN C;
<C>[^ab\n] BEGIN DEAD;
<C>\n BEGIN INITIAL; {printf("Accepted\n");}
<D>b BEGIN G;
<D>a BEGIN E;
<D>[^ab\n] BEGIN DEAD;
<D>\n BEGIN INITIAL; {printf("Accepted\n");}
<E>b BEGIN F;
<E>a BEGIN B;
<E>[^ab\n] BEGIN DEAD;
<E>\n BEGIN INITIAL; {printf("Accepted\n");}
<F>b BEGIN G;
<F>a BEGIN E; <F>[^ab\n] BEGIN DEAD;

<F>\n BEGIN INITIAL; {printf("Not Accepted\n");}
<G>b BEGIN INITIAL;
<G>a BEGIN A;
<G>[^ab\n] BEGIN DEAD;
<G>\n BEGIN INITIAL; {printf("Accepted\n");}
<DEAD>[^\n] BEGIN DEAD;
<DEAD>\n BEGIN INITIAL; {printf("Invalid\n");}
%%
int yywrap() {
return 1; }
int main() {
printf("Enter String\n"); yylex();
return 0;
}

-------------------

11)

%{
 %}
%s A B DEAD
%%
 <INITIAL>1 BEGIN A;
<INITIAL>0 BEGIN INITIAL;
<INITIAL>[^01\n] BEGIN DEAD;
<INITIAL>\n BEGIN INITIAL; {printf("Not Accepted\n");}
<A>1 BEGIN B;
<A>0 BEGIN INITIAL;
<A>[^01\n] BEGIN DEAD;
<A>\n BEGIN INITIAL; {printf("Not Accepted\n");}
<B>1 BEGIN B;
<B>0 BEGIN INITIAL;
<B>[^01\n] BEGIN DEAD;
<B>\n BEGIN INITIAL; {printf("Accepted\n");}
<DEAD>[^\n] BEGIN DEAD;
<DEAD>\n BEGIN INITIAL; {printf("Invalid\n");}
 %%
intmain() {
printf("Enter String\n");
yylex();
return0;
}

----------------------

12)

%{ %}
%s A DEAD
%%
<INITIAL>a BEGIN A;
<INITIAL>b BEGIN INITIAL;
<INITIAL>[^ab\n] BEGIN DEAD;
<INITIAL>\n BEGIN INITIAL; {printf("Accepted\n");}
<A>a BEGIN INITIAL;
<A>b BEGIN A;
<A>[^ab\n] BEGIN DEAD;
<A>\n BEGIN INITIAL; {printf("Not Accepted\n");}
<DEAD>[^\n] BEGIN DEAD;
<DEAD>\n BEGIN INITIAL; {printf("Invalid\n");}
%%
int yywrap() {
return 1;
}
int main() {
printf("Enter String\n");

yylex(); 
return 0; 
}

--------------

13)

%{ %}
%s A B C DEAD
%%
<INITIAL>1 BEGIN A;
<INITIAL>0 BEGIN B;
<INITIAL>[^01\n] BEGIN DEAD;
<INITIAL>\n BEGIN INITIAL; {printf("Not Accepted\n");}
<A>1 BEGIN INITIAL;
<A>0 BEGIN C;
<A>[^01\n] BEGIN DEAD;
<A>\n BEGIN INITIAL; {printf("Not Accepted\n");}
<B>1 BEGIN C;
<B>0 BEGIN INITIAL;
<B>[^01\n] BEGIN DEAD;
<B>\n BEGIN INITIAL; {printf("Accepted\n");}
<C>1 BEGIN B;
<C>0 BEGIN A;
<C>[^01\n] BEGIN DEAD;
<C>\n BEGIN INITIAL; {printf("Not Accepted\n");}
<DEAD>[^\n] BEGIN DEAD;
<DEAD>\n BEGIN INITIAL; {printf("Invalid\n");}
%%
int main() {
printf("Enter String\n"); yylex();
return 0;
}

--------------

14)

%{ %}
%s A B C DEAD
 %%
<INITIAL>[0-9]+ BEGIN A;
<INITIAL>[0-9]+[.][0-9]+ BEGIN B; <INITIAL>[A-Za-z_][A-Za-z0-9_]* BEGIN C; <INITIAL>[^\n] BEGIN DEAD;
<INITIAL>\n BEGIN INITIAL; {printf("Not Accepted\n");}
<A>[^\n] BEGIN DEAD;
<A>\n BEGIN INITIAL; {printf("Integer\n");}
<B>[^\n] BEGIN DEAD;
<B>\n BEGIN INITIAL; {printf("Float\n");}
<C>[^\n] BEGIN DEAD;
<C>\n BEGIN INITIAL; {printf("Identifier\n");}
 <DEAD>[^\n] BEGIN DEAD;
<DEAD>\n BEGIN INITIAL; {printf("Invalid\n");}
%%
intyywrap() {
return1; }
 intmain() {
printf("Enter String\n"); yylex();
return0;
}
 






