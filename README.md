# SISTEMA-QUE-GUARDA-LINKS-UTEIS-POR-ASSUNTO

import java.util.Scanner;
import java.util.ArrayList;
import java.io.*;

class Link {
    String assunto;
    String url;

    Link(String assunto, String url) {
        this.assunto = assunto;
        this.url = url;
    }
}

public class SistemaDeLinks {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        ArrayList<Link> listaLinks = new ArrayList<>();

        carregarArquivo(listaLinks);

        int opcao;

        do {
            System.out.println("\nMenu:");
            System.out.println("1. Adicionar link");
            System.out.println("2. Editar link");
            System.out.println("3. Excluir link");
            System.out.println("4. Listar links por assunto");
            System.out.println("5. Sair");
            System.out.print("Escolha uma opção: ");
            opcao = scanner.nextInt();
            scanner.nextLine();

            switch (opcao) {
                case 1:
                    System.out.println("Insira o assunto do link: ");
                    String assunto = scanner.nextLine();

                    System.out.println("Insira a URL do link: ");
                    String url = scanner.nextLine();

                    listaLinks.add(new Link(assunto, url));
                    break;

                case 2:
                    System.out.println("Insira o número do link a editar (1-" + listaLinks.size() + "):");
                    int linkEditar = scanner.nextInt();
                    scanner.nextLine();

                    System.out.println("Insira o novo assunto do link: ");
                    String novoAssunto = scanner.nextLine();

                    System.out.println("Insira a nova URL do link: ");
                    String novaUrl = scanner.nextLine();

                    listaLinks.set(linkEditar - 1, new Link(novoAssunto, novaUrl));
                    break;

                case 3:
                    System.out.println("Insira o número do link a excluir (1-" + listaLinks.size() + "):");
                    int linkExcluir = scanner.nextInt();

                    listaLinks.remove(linkExcluir - 1);
                    break;

                case 4:
                    System.out.println("Insira o assunto para listar os links: ");
                    String assuntoParaListar = scanner.nextLine();
                    listarLinksPorAssunto(listaLinks, assuntoParaListar);
                    break;

                case 5:
                    System.out.println("Saindo...");
                    scanner.close();
                    break;

                default:
                    System.out.println("Opção inválida!");
                    break;
            }
        } while (opcao != 5);

        salvarArquivo(listaLinks);
    }

    public static void salvarArquivo(ArrayList<Link> listaLinks) {
        try {
            BufferedWriter writer = new BufferedWriter(new FileWriter("links.txt"));
            for (Link link : listaLinks) {
                writer.write("***Assunto***\n" + link.assunto + "\n***URL***\n" + link.url + "\n");
            }
            writer.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    public static void carregarArquivo(ArrayList<Link> listaLinks) {
        try {
            BufferedReader reader = new BufferedReader(new FileReader("links.txt"));
            String line = reader.readLine();
            while (line != null) {
                if (line.equals("***Assunto***")) {
                    String assunto = reader.readLine();
                    reader.readLine(); // pula a linha da URL
                    String url = reader.readLine();
                    listaLinks.add(new Link(assunto, url));
                }
                line = reader.readLine();
            }
            reader.close();
        } catch (FileNotFoundException e) {
            // O arquivo não existe ainda, então vamos ignorar essa exceção.
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    public static void listarLinksPorAssunto(ArrayList<Link> listaLinks, String assunto) {
        System.out.println("Links para o assunto '" + assunto + "':");
        for (Link link : listaLinks) {
            if (link.assunto.equalsIgnoreCase(assunto)) {
                System.out.println("URL: " + link.url);
            }
        }
    }
}
