# 🔍 Scan de Portas com Nmap - Roteador Doméstico (Minha Rede)

## 📌 Objetivo
Realizar um scan de portas TCP em um roteador doméstico (TP-Link Archer C5) utilizando o Nmap, identificando serviços ativos e analisando possíveis riscos de segurança.

## 🛠️ Ferramentas Utilizadas
- **Ubuntu**
- **Nmap 7.94**

## 🖥️ Alvo
- **IP:** 192.168.0.1 (Gateway)
- **Fabricante:** TP-Link Technologies
- **MAC:** B0:95:75:A3:E3:B1

## 📡 Comando Executado
```bash
nmap -sS 192.168.0.1
```
## ​​​​​​🟰​ Resultado da Scan.
```
|Porta | Estado | Serviço | Risco|
|21/tcp | Aberta | FTP | Médio|
|23/tcp| Aberta | Telnet | Alto|
|53/tcp | Aberta | DNS Baixo|
|80/tcp | Aberta | HTTP | Baixo|
|139/tcp | Aberta | NetBIOS | Médio|
|445/tcp | Aberta | Microsoft-DS | Alto|
|1900/tcp | Aberta | UPnP | Médio|
```

## ​​​​🧠​ Análise de Segurança

• Telnet (23): Protocolo sem criptografia.
Se exposto, permite interceptação de
credenciais.

•FTP (21): Pode permitir transferência de
arquivos sem criptografia.

• SMB (445): Alvo comum de ransomwares (ex:Recomendações
WannaCry).

•UPnP (1900): Pode permitir que dispositivos
abram portas no roteador sem autorização.

## ​​​​​✅​ Recomendações

• Desativar Telnet e FTP se não forem
utilizados.

· Verificar se o UPnP é necessário.

• Manter o firmware do roteador atualizado.

•Utilizar SSH em vez de Telnet, quando
possível.

## ​​​​​✅📚​Aprendizados

• Identificação de serviços ativos em um
dispositivo de rede.

• Análise de riscos baseada em portas abertas.

•Importância de fechar serviços
desnecessários.

## ​​​​​🔗​Referências

https://nmap.org/book/
https://www.iana.org/assignments/service-names-port-numbers
