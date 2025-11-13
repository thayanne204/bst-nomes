# bst-nomes
//Casse No
public class No {
	String nome;
	No esquerda, direita;
	int contador; // Para contar repetições

	No(String nome) {
		this.nome = nome;
		this.contador = 1;
	}
}

//Classe ArvoreNomes
public class ArvoreNomes {private No raiz;
private int totalNomes; // Para contar o total de nomes (incluindo repetições)

// Inserir nome na árvore
public void inserir(String nome) {
    raiz = inserirRecursivo(raiz, nome);
}

private No inserirRecursivo(No no, String nome) {
    if (no == null) {
        totalNomes++;
        return new No(nome);
    }

    int comparacao = nome.compareToIgnoreCase(no.nome);

    if (comparacao < 0) {
        no.esquerda = inserirRecursivo(no.esquerda, nome);
    } else if (comparacao > 0) {
        no.direita = inserirRecursivo(no.direita, nome);
    } else {
        // Nome já existe, incrementa contador
        no.contador++;
        totalNomes++;
    }

    return no;
}

// Listar nomes em ordem alfabética
public void listarEmOrdem() {
    System.out.println("Nomes em ordem alfabética:");
    listarEmOrdemRecursivo(raiz);
    System.out.println();
}

private void listarEmOrdemRecursivo(No no) {
    if (no != null) {
        listarEmOrdemRecursivo(no.esquerda);
        System.out.print(no.nome);
        if (no.contador > 1) {
            System.out.print(" (" + no.contador + " vezes)");
        }
        System.out.print(" ");
        listarEmOrdemRecursivo(no.direita);
    }
}

// Listar nomes em ordem decrescente
public void listarReversa() {
    System.out.println("Nomes em ordem decrescente:");
    listarReversaRecursivo(raiz);
    System.out.println();
}

private void listarReversaRecursivo(No no) {
    if (no != null) {
        listarReversaRecursivo(no.direita);
        System.out.print(no.nome);
        if (no.contador > 1) {
            System.out.print(" (" + no.contador + " vezes)");
        }
        System.out.print(" ");
        listarReversaRecursivo(no.esquerda);
    }
}

// Buscar um nome específico
public boolean buscar(String nome) {
    return buscarRecursivo(raiz, nome);
}

private boolean buscarRecursivo(No no, String nome) {
    if (no == null) {
        return false;
    }

    int comparacao = nome.compareToIgnoreCase(no.nome);

    if (comparacao < 0) {
        return buscarRecursivo(no.esquerda, nome);
    } else if (comparacao > 0) {
        return buscarRecursivo(no.direita, nome);
    } else {
        return true; // Nome encontrado
    }
}

// Contar nomes únicos (nós distintos)
public int contarNomesUnicos() {
    return contarNodosRecursivo(raiz);
}

private int contarNodosRecursivo(No no) {
    if (no == null) {
        return 0;
    }
    return 1 + contarNodosRecursivo(no.esquerda) + contarNodosRecursivo(no.direita);
}

// Contar total de nomes (incluindo repetições)
public int contarTotalNomes() {
    return totalNomes;
}

// Método para obter informações detalhadas sobre um nome
public void informacoesNome(String nome) {
    No no = buscarNo(raiz, nome);
    if (no != null) {
        System.out.println("Nome: " + no.nome);
        System.out.println("Quantidade: " + no.contador);
    } else {
        System.out.println("Nome '" + nome + "' não encontrado.");
    }
}

private No buscarNo(No no, String nome) {
    if (no == null) {
        return null;
    }

    int comparacao = nome.compareToIgnoreCase(no.nome);

    if (comparacao < 0) {
        return buscarNo(no.esquerda, nome);
    } else if (comparacao > 0) {
        return buscarNo(no.direita, nome);
    } else {
        return no;
    }
}
}
//Classe Main
public class Main { public static void main(String[] args) {
    ArvoreNomes arvore = new ArvoreNomes();

    // Inserindo nomes (incluindo repetições)
    System.out.println("Inserindo nomes...");
    String[] nomes = {"Maria", "Ana", "João", "Pedro", "Bruno", "Carla", "Ana", "João", "Maria"};
    
    for (String nome : nomes) {
        arvore.inserir(nome);
    }

    // Testando as funcionalidades
    System.out.println("=== TESTES DA ÁRVORE DE NOMES ===\n");

    // 1. Listar em ordem alfabética
    arvore.listarEmOrdem();

    // 2. Listar em ordem decrescente
    arvore.listarReversa();

    // 3. Buscar nomes
    System.out.println("Buscando nomes:");
    String[] busca = {"João", "Lucas", "Ana", "Carlos"};
    for (String nome : busca) {
        boolean encontrado = arvore.buscar(nome);
        System.out.println("'" + nome + "' encontrado: " + encontrado);
    }
    System.out.println();

    // 4. Contar nomes
    System.out.println("Estatísticas:");
    System.out.println("Nomes únicos: " + arvore.contarNomesUnicos());
    System.out.println("Total de nomes (com repetições): " + arvore.contarTotalNomes());
    System.out.println();

    // 5. Informações detalhadas
    System.out.println("Informações detalhadas:");
    arvore.informacoesNome("Ana");
    arvore.informacoesNome("Maria");
    arvore.informacoesNome("Lucas");
}
}
