# estrutura
Estrutural emC


#include <stdio.h>
#include <malloc.h>
#include <stdlib.h>
#define MAX 50

typedef int boo;
typedef int TIPOCHAVE;



typedef struct {
TIPOCHAVE chave;
int salario;
} REGISTRO;

typedef struct aux {
REGISTRO reg;
struct aux* prox;
} ELEMENTO;

typedef ELEMENTO* PONT;
typedef struct {
REGISTRO X[MAX];
PONT inicio;
} LISTA;


void inicializarLista(LISTA* l);
void exibirLista(LISTA* l);
int tamanho(LISTA* l);
PONT buscaSequencial(LISTA* l, TIPOCHAVE ch);
PONT buscaSeqOrd(LISTA* l, TIPOCHAVE ch);
PONT buscaSequencialExc(LISTA* l, TIPOCHAVE ch, PONT* ant);
boo inserirElemListaOrd(LISTA* l, REGISTRO reg);
boo excluirElemLista(LISTA* l, TIPOCHAVE ch);
REGISTRO novoRegistro();




int main(){
	
	int op = 0, posicaoNaLista, tamanhoDaLista;
	PONT localizacao;
	TIPOCHAVE matricula;
	int salario;
	bool inserido = true;
	bool exclusao = false;
	char resposta;
	LISTA listaDeProducao;
	LISTA* enderecoLista = &listaDeProducao;
	REGISTRO novo_reg;
	
	
	do{
		
		printf("----------------------------------------------------\n");
			printf("Selecione uma opcao\n");
			printf("---------------------------------------------------\n");
			printf("1 - Inicializar Lista\n");
			printf("2 - Exibir tamanho da lista\n");
			printf("3 - Exibir lista\n");
			printf("4 - Localizar elemento na Lista\n");
			printf("5 - Inserir elemeno na lista em posicao especifica\n");
			printf("6 - Excluir elemento especifico da Lista\n");
			printf("7 - sair\n");
			
			
			
			scanf("%i", &op);
			
			switch(op)
			{
				case 1:
					inicializarLista(enderecoLista);
				break;
			
				case 2: tamanhoDaLista = tamanho(enderecoLista);
				if (tamanhoDaLista == 1){
					printf("A lista tem %i registro! \n", tamanhoDaLista);
				}else
					printf("A lista tem %i registros!! \n", tamanhoDaLista);
				break;
				
				case 3: exibirLista(enderecoLista);
				break;
				
				
				case 4: printf("Qual matricula esta buscando? \n");
				scanf("%i", &matricula);
				
				localizacao = buscaSeqOrd(enderecoLista, matricula);
			
				if (localizacao == 0){
					printf("Nao a registro!\n");
				}else
					printf("Registro localizado na posicao %i \n", localizacao);
				break;
				
				case 5: printf("Onde deseja posicionar seu elemento?\n");
						scanf("%i", &posicaoNaLista);
						
						novo_reg = novoRegistro();
						
						inserido = inserirElemListaOrd(enderecoLista, novo_reg);
						
						if(inserido == true){
							printf("Registrado com sucesso!\n");
						}else
						printf("Registro nao realizado!\n");
						break;
				case 6: printf("Qual matricula sera excluida?\n");
						scanf("%i", &matricula);
					 	
						
					
						if(exclusao == true){
							printf("Excluido com sucesso\n");
						}else
							printf("Registro inexistente\n");
						break;
				}
				
				printf("Deseja continuar? s - SIM, n - NAO!!! \n");
				scanf("%s", &resposta);
				system("cls");
		
	}while(resposta=='s');	
	
}


REGISTRO novoRegistro(){
	int salario;
	int chave;
	
	printf("Digite o salario: \n");
	scanf("%i",&salario);
				
	printf("Digite uma chave: \n");
	scanf("%i", &chave);
	
	REGISTRO novo_reg;
	novo_reg.salario = salario;
	novo_reg.chave = chave;
	
	return novo_reg;
}




void inicializarLista(LISTA* l){
	l->inicio = NULL;
	printf("lista inicializada\n");
}


void exibirLista(LISTA* l){
	PONT end = l->inicio;
	printf("Lista: \" ");
	while (end != NULL) {
	printf("%i ", end->reg.salario);	
	printf("%i \n", end->reg.chave);
	
	end = end->prox;
	}
}

int tamanho(LISTA* l) {
		PONT end = l->inicio;
		int tam = 0;
	while (end != NULL) {
	tam++;
	end = end->prox;
	}
return tam;
}

PONT buscaSequencial(LISTA* l, TIPOCHAVE ch) { 
PONT pos = l->inicio;
 	while (pos != NULL) { 
 	if (pos->reg.chave == ch) return pos; pos = pos->prox;
  	} 
  	return NULL; 
}


PONT buscaSeqOrd(LISTA* l, TIPOCHAVE ch) {
	PONT pos = l->inicio;
	while (pos != NULL && pos->reg.chave < ch) pos = pos->prox;
	if (pos != NULL && pos->reg.chave == ch) return pos; 
	return NULL;
}

PONT buscaSequencialExc(LISTA* l, TIPOCHAVE ch, PONT* ant){
	*ant = NULL;
	PONT atual = l->inicio;
	while ((atual != NULL) && (atual->reg.chave<ch)) {
	*ant = atual;
	atual = atual->prox;
	}
	if ((atual != NULL) && (atual->reg.chave == ch)) return atual;
	return NULL;
}


boo inserirElemListaOrd(LISTA* l, REGISTRO reg) {
	TIPOCHAVE ch = reg.chave;
	PONT ant, i;
	i = buscaSequencialExc(l,ch,&ant);
	if (i != NULL) return false;
	i = (PONT) malloc(sizeof(ELEMENTO));
	i->reg = reg;
	if (ant == NULL) {
	i->prox = l->inicio;
	l->inicio = i;
	} else {
	i->prox = ant->prox;
	ant->prox = i;
	}
	return true;
}

boo excluirElemLista(LISTA* l, TIPOCHAVE ch) {
	PONT ant, i;
	i = buscaSequencialExc(l,ch,&ant);
	if (i == NULL) return false;
	if (ant == NULL) l->inicio = i->prox;
	else ant->prox = i->prox;
	free(i);
	return true;
}










