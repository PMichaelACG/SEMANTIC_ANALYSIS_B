sudo apt-get update
sudo apt-get install -y build-essential flex bison graphviz python3 unzip
export LC_ALL=C
mkdir -p build
bison -Wall -Werror -d --report=state -o build/parser.tab.c parser.y
flex -o build/lex.yy.c scanner.l

gcc -std=c11 -Wall -Wextra -Werror -pedantic -g -I. -Ibuild -D_POSIX_C_SOURCE=200809L ast.c ast_dot.c semantic.c semantic_dot.c main.c build/parser.tab.c build/lex.yy.c -o build/quectoc-ast
./build/quectoc-ast --semantic examples/semantic_sample.qc > build/typed.dot
dot -Tsvg build/typed.dot -o build/typed.svg
