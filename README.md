# Dúvidas em java e como resolve-las
Repositório criado para funcionar como backup de resolução de dúvidas/dicas

Remover virgula/ponto e zeros desnecessários de uma string com regex

String numeroContrato = "123.0";
String novaString = numeroContrato.replace("/(^0+(?=\\d))|(.?0+$)/g","");
//Saida 123


Remover dois ultimos numeros de uma string
String nome = 1213.0;
nome.substring(0,nome.length()-2)
//Saida 1213
