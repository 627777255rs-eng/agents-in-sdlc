---
title: "Lição 12 - Implante o site com GitHub Pages"
description: "Use o aplicativo GitHub Copilot para configurar um fluxo de trabalho do Pages, revise o pull request e confirme o Space Quiz publicado."
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

Dê ao Space Quiz uma presença pública implantando-o com GitHub Pages. O Copilot pode preparar o fluxo de trabalho e orientar a implantação, mas você continua responsável por revisar as alterações e confirmar que o site está publicado.

Nesta lição, você:

- solicitará ao Copilot que configure uma implantação no GitHub Pages;
- revisará o fluxo de trabalho e o pull request antes do merge;
- acompanhará a implantação e verificará o site publicado.

## Cenário

O Space Quiz já está em um repositório do GitHub, mas as pessoas ainda precisam de uma maneira simples de experimentá-lo. Publicar o site de um único arquivo com GitHub Pages o torna disponível por meio de uma URL pública, sem adicionar servidor ou dependência de tempo de execução.

## Solicite ao Copilot que prepare a implantação

> [!WARNING]
> Criar um pull request pode confirmar e enviar código. Revise suas configurações de aprovação e o repositório de destino antes de enviar este prompt. Com **Approve all**, não espere um prompt de confirmação separado.

Na sessão do aplicativo Copilot para `space-quiz`, envie este prompt:

```plaintext
Prepare a GitHub Pages deployment for this repository. Create a GitHub Actions workflow that builds the website if needed and deploys it to GitHub Pages after changes reach the default branch. Add a README describing the app and how to run it locally if one does not already exist. Create a pull request with these changes and report its link and any required Pages settings or permissions. Do not merge, enable auto-merge, change Pages settings, or deploy. Stop and wait for my review.
```

## Revise o fluxo de trabalho e o pull request

1. Confirme que o README será adicionado somente se o repositório ainda não tiver um.
2. Revise as alterações do pull request, incluindo as etapas de build e implantação do fluxo de trabalho, os acionadores e as permissões.
3. Confirme que o fluxo de trabalho implanta a partir da branch padrão, não de um pull request não integrado.
4. Solicite correções e revise o diff atualizado. Aguarde a conclusão das verificações obrigatórias do pull request antes de autorizar o merge.

## Autorize o merge e a implantação

> [!WARNING]
> O prompt a seguir autoriza o agente a integrar o pull request revisado, alterar as configurações do Pages do repositório e publicar o site. Envie-o somente após concluir sua revisão e confirmar o repositório e o pull request. As configurações de aprovação podem permitir essas ações sem outro prompt de confirmação.

Substitua `<PR_NUMBER>` pelo número do pull request que você revisou e envie este prompt separado:

```plaintext
I have reviewed pull request <PR_NUMBER> in this repository and authorize its merge and deployment. Enable GitHub Pages using GitHub Actions, merge that pull request only after required checks pass, and monitor the deployment until it succeeds. Report the final URL of the live website. If you lack permission to enable Pages or merge, tell me exactly what I need to approve or do; do not report success until the site is live.
```

![Diagrama de três etapas: um pull request mesclado do space-quiz; uma execução bem-sucedida do GitHub Actions Deploy website com as etapas Build website, Upload artifact e Deploy to Pages; e o Space Quiz publicado em yourname.github.io/space-quiz/.](../../../_images/first-steps-app-pages-deploy.svg)

## Verifique a implantação

1. Acompanhe a execução de implantação do GitHub Actions até que ela seja concluída com sucesso.
2. Abra a URL informada e confirme que o Space Quiz é carregado.

Se o agente não puder ativar o Pages ou integrar o pull request, siga as instruções específicas fornecidas. Não considere a tarefa concluída até que a implantação seja bem-sucedida e o site publicado seja carregado.

## Resumo e próximos passos

Seu Space Quiz agora está disponível como um site publicado, e você confirmou a implantação em vez de depender apenas do relatório do agente. Continue para a [Lição 13: Revisão e próximos passos][next-lesson].

[next-lesson]: ../13-review/
