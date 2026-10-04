# Segurança e privacidade

Este projeto é uma aplicação web estática e não deve receber credenciais, tokens, chaves de API ou dados pessoais.

## Não publique

- arquivos `.env`;
- tokens de plataformas;
- dados pessoais de audiência;
- relatórios acadêmicos reservados;
- métricas privadas sem autorização;
- material artístico de terceiros sem licença.

A persistência atual ocorre no navegador do usuário por meio de `localStorage`. Antes de usar o laboratório com dados reais, remova informações que possam identificar pessoas e verifique as regras das plataformas envolvidas.

Para reportar uma falha de segurança, abra uma issue sem expor publicamente segredos ou dados pessoais.
