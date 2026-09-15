# Croqui de Atracação JBS — configuração e publicação

## Arquivos

- `croqui_atracacao_jbs.html`: aplicação completa, com desenhos incorporados. Publique como `index.html`.
- `firestore.rules`: regras para usuários autorizados.

O layout, biblioteca, desenho em escala, amarrações, arraste, edição/duplicação/remoção dos navios e impressão foram mantidos. Os desenhos aprovados guindaste-sts.svg e guindaste-mhc.svg foram incorporados ao HTML, no croqui e na legenda, sem depender de arquivos externos.

## 1. Firebase

1. Crie um projeto no console Firebase e registre um aplicativo Web.
2. Crie o banco **Cloud Firestore padrão**, escolhendo a região adequada à equipe. Publique o conteúdo de `firestore.rules` na aba Regras. Não use regras de acesso público.
3. Ative **Authentication → Sign-in method → Email/Password**. Crie as contas da equipe em **Users** e anote os respectivos UIDs. Não há cadastro público no aplicativo.
4. No Firestore, crie a coleção `metadata`, documento `access`, campo `uids` do tipo **array**, contendo os UIDs autorizados como strings. Apenas o administrador mantém essa lista no console. As regras podem consultá-la mesmo sem conceder leitura direta desse documento aos usuários.
5. Em Configurações do projeto → Seus apps, copie o objeto de configuração Web. No HTML, procure `const firebaseConfig` e substitua os valores. Não coloque senha, chave privada nem credenciais de conta de serviço no HTML. A configuração Web identifica o projeto; a proteção é feita pelas regras e pelo login.
6. Em Authentication → Settings → Authorized domains, adicione `SEU_USUARIO.github.io` e o domínio personalizado, se houver. Para testes por servidor local, adicione `localhost`.
7. Abra a aplicação por HTTPS ou servidor local, clique em **Entrar** e use uma conta autorizada. Aguarde **Sincronizado**.
8. Em **Configurações**, confirme parâmetros e clique em **Publicar configurações globais**. Cadastre os navios na biblioteca compartilhada. Os exemplos locais não são importados automaticamente para um banco vazio.

## 2. GitHub Pages

1. Crie um repositório e envie o HTML com o nome `index.html` para a raiz da branch `main`.
2. Em Settings → Pages → Build and deployment, selecione **Deploy from a branch**, branch `main`, pasta `/ (root)` e salve.
3. Abra o endereço indicado pelo GitHub após o término da publicação. Não é necessário servidor próprio nem etapa de compilação.
4. Para atualizar a aplicação, substitua `index.html` e confirme o envio. A publicação pode demorar alguns minutos.

## Dados compartilhados

| Caminho | Conteúdo |
|---|---|
| `vessels/{id}` | Nome, LOA, boca, revisão e autoria/data de atualização |
| `settings/global` | Comprimento, distância mínima, cabeços, defensas, seis guindastes e limites dos berços |
| `sketches/{id}` | Data, referência, sentido, observações, navios, posições, orientações, cabos e cópia dos parâmetros/guindastes utilizados |
| `metadata/app` | Versão do esquema e autoria/data da última gravação; criado automaticamente na primeira gravação |
| `metadata/access` | Lista administrativa de UIDs autorizados |

Biblioteca, configurações e lista de croquis usam atualizações em tempo real. Os croquis abertos preservam seu desenho e os parâmetros salvos. Para adotar novos padrões compartilhados, use **Configurações → Aplicar globais ao croqui**. Movimentar um guindaste altera o croqui local; **Salvar croqui** guarda sua posição nesse croqui e **Publicar configurações globais** compartilha essas posições como padrão.

## Salvamento, erros e concorrência

- Editar/arrastar não envia o croqui. Só **Salvar croqui** ou **Salvar cópia** inicia a gravação.
- **Salvar croqui** atualiza o registro aberto; **Salvar cópia** cria outro registro.
- **Abrir / excluir** permite reabrir e remover registros compartilhados, com confirmação antes da exclusão.
- As gravações usam transações e revisões. Se outra pessoa modificar ou excluir o mesmo registro, o aplicativo impede a sobrescrita e orienta a reabrir ou salvar uma cópia.
- **Sincronizado** significa que os quatro fluxos de dados foram confirmados pelo servidor. O indicador separado de rascunho informa alterações ainda não salvas. **Salvando** permanece durante a transação; falhas têm mensagem própria.
- Sem conexão, não há fila automática de gravações. O rascunho é guardado localmente e o usuário deve tentar salvar novamente quando conectado. **Recuperar rascunho** recupera o último rascunho desse navegador/projeto.
- O cache local não substitui o Firestore. Pode ser removido pelo navegador e não é backup. O SDK usa cache em memória; a contingência local é explícita. A primeira abertura do site e o carregamento do SDK exigem internet; não é um aplicativo instalado para iniciar totalmente offline.
- O cache permanece no navegador após sair. Em computadores compartilhados, use perfis individuais ou remova os dados do site ao terminar.

## Berços e referências físicas

O anexo não fornece confirmação confiável dos quatro intervalos. Por isso, o desenho inicia com B1–B4 pendentes, sem divisórias ou comprimentos inferidos. Em Configurações, informe início e fim dos quatro berços em ordem crescente, sem sobreposição. Intervalos não precisam preencher artificialmente os 1.030 m. A tabela indica todos os berços interceptados pelo navio.

O comprimento inicial de 1.030 m, posições de guindastes, cabeços e defensas vêm do HTML original. O cabeço registrado em 1.031 m é preservado nos dados, mas está fora do cais inicial e não é desenhado; valide essa discrepância com a referência operacional. Todas as defensas do anexo agora são desenhadas, em vez de uma a cada duas. Enquanto faltarem os limites, a validação informa que é parcial, inclusive no PDF.

## Teste antes do uso operacional

1. Entre em dois navegadores com contas autorizadas. Cadastre, edite e exclua um navio na biblioteca e confira a atualização no outro navegador.
2. Publique configurações e confirme sua disponibilidade no outro navegador. Aplique-as explicitamente ao croqui em edição.
3. Crie um croqui, mova navios/guindastes, conecte cabos, escreva observações. Antes de salvar, ele não deve aparecer no outro navegador; depois de salvar, abra-o e compare os dados.
4. Atualize e exclua um croqui. Abra o mesmo croqui nos dois navegadores, salve em um e tente salvar no outro: deve aparecer conflito sem sobrescrever.
5. Desconecte a rede, edite, tente salvar e recupere o rascunho. Reconecte e salve explicitamente.
6. Entre com uma conta cujo UID não consta na lista: as leituras/gravações devem ser negadas. Teste a remoção de autorização pelo console.
7. Confira escala, proa/popa, limites, distâncias, cabos e impressão/PDF com uma operação conhecida antes da publicação operacional.

A integração real depende de configurar o projeto e as contas acima. O arquivo não inclui credenciais de um projeto e não foi publicado em uma conta externa.

## Referências oficiais

- Firebase Web: https://firebase.google.com/docs/web/setup
- Atualizações em tempo real: https://firebase.google.com/docs/firestore/query-data/listen
- Transações e conflitos: https://firebase.google.com/docs/firestore/manage-data/transactions
- Cache: https://firebase.google.com/docs/firestore/manage-data/enable-offline
- Publicação no GitHub Pages: https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site
