# deploy-spa-cpanel
Documentação de um problema real de deploy SPA em hospedagem cPanel (Apache)
# Deploy de site SPA em hospedagem cPanel (Apache)

## 📌 Contexto
Este repositório documenta a resolução de um problema real ocorrido durante o deploy
de um site desenvolvido com a plataforma Lovable, hospedado em servidor Apache via cPanel.

## ❌ Problema
Após o deploy, ao acessar URLs diretamente (ex: /sobre, /galeria),
o site retornava erro **404 – Página não encontrada**.
## 📷 Evidência do erro

![Erro 404](erro%20404%20IBDESP.png)


A página inicial funcionava normalmente.



![Página inicial funcionando](P%C3%A1gina%20Inicial%20IBDESP.png)



## 🖥️ Ambiente
- Hospedagem: HostGator
- Painel: cPanel
- Servidor: Apache
- Tipo de site: SPA (Single Page Application)

## 🔍 Causa
O Apache tentava localizar pastas físicas correspondentes às rotas acessadas.
Como se trata de uma SPA, o roteamento é feito no frontend,
e essas pastas não existem no servidor.

## ✅ Solução
Criação do arquivo `.htaccess` com regras de rewrite para redirecionar
todas as requisições para o `index.html`, permitindo que o frontend
gerencie as rotas corretamente.

```apache
<IfModule mod_rewrite.c>
  RewriteEngine On
  RewriteBase /

  RewriteRule ^index\.html$ - [L]

  RewriteCond %{REQUEST_FILENAME} !-f
  RewriteCond %{REQUEST_FILENAME} !-d
  RewriteRule . /index.html [L]
</IfModule>
