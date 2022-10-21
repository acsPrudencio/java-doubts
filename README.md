# Dúvidas em java e como resolve-las
Repositório criado para funcionar como backup de resolução de dúvidas/dicas

Remover virgula/ponto e zeros desnecessários de uma string com regex

String numeroContrato = "123.0";
String novaString = numeroContrato.replace("/(^0+(?=\\d))|(.?0+$)/g","");
System.out.println(novaString);
