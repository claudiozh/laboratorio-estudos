# 📩 Template envio de e-mail

```html
<!doctype html>
<html>

<head>
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@400;500;600&display=swap" rel="stylesheet">
  <title>Ativação de conta</title>
  <style>
    /* -------------------------------------
          GLOBAL RESETS
      ------------------------------------- */

    /*All the styling goes here*/

    img {
      border: none;
      -ms-interpolation-mode: bicubic;
      max-width: 100%;
    }

    body {
      background-color: #F1F4F8;
      font-family: 'Poppins', 'Poppins', sans-serif;
      -webkit-font-smoothing: antialiased;
      font-size: 14px;
      line-height: 1.6;
      margin: 0;
      padding: 0;
      -ms-text-size-adjust: 100%;
      -webkit-text-size-adjust: 100%;
    }

    table {
      border-collapse: separate;
      mso-table-lspace: 0pt;
      mso-table-rspace: 0pt;
      width: 100%;
    }

    table td {
      font-family: 'Poppins', sans-serif;
      font-size: 14px;
      vertical-align: top;
    }

    /* -------------------------------------
          BODY & CONTAINER
      ------------------------------------- */

    .body {
      background-color: #F1F4F8;
      width: 100%;
    }

    /* Set a max-width, and make it display as block so it will automatically stretch to that width, but will also shrink down on a phone or something */
    .container {
      display: block;
      margin: 0 auto !important;
      /* makes it centered */
      max-width: 550px;
      padding: 10px;
      width: 550px;
    }

    /* This should also be a block element, so that it will fill 100% of the .container */
    .content {
      box-sizing: border-box;
      display: block;
      margin: 0 auto;
      max-width: 550px;
      padding: 10px;
    }

    /* -------------------------------------
          HEADER, FOOTER, MAIN
      ------------------------------------- */
    .main {
      background: #ffffff;
      border-radius: 10px;
      width: 100%;
    }

    .wrapper {
      box-sizing: border-box;
      padding: 20px;
    }

    .content-block {
      padding-bottom: 10px;
      padding-top: 10px;
    }

    .footer {
      clear: both;
      margin-top: 10px;
      text-align: center;
      width: 100%;
    }

    .footer td,
    .footer p,
    .footer span,
    .footer a {
      color: #999999;
      font-size: 12px;
      text-align: center;
    }

    /* -------------------------------------
          TYPOGRAPHY
      ------------------------------------- */
    h1,
    h2,
    h3,
    h4 {
      color: #000000;
      font-family: 'Poppins', sans-serif;
      font-weight: 400;
      line-height: 1.4;
      margin: 0;
      margin-bottom: 15px;
    }

    h1 {
      font-weight: bold;
      font-size: 18px;
    }

    p,
    ul,
    ol {
      font-family: 'Poppins', sans-serif;
      font-size: 14px;
      font-weight: normal;
      margin: 0;
      margin-bottom: 15px;
    }

    span {
      font-size: 12px;
      color: #999999;
    }

    p li,
    ul li,
    ol li {
      list-style-position: inside;
      margin-left: 5px;
    }

    a {
      color: #3498db;
      text-decoration: underline;
    }

    /* -------------------------------------
          OTHER STYLES THAT MIGHT BE USEFUL
      ------------------------------------- */
    .last {
      margin-bottom: 0;
    }

    .first {
      margin-top: 0;
    }

    .align-center {
      text-align: center;
    }

    .align-right {
      text-align: right;
    }

    .align-left {
      text-align: left;
    }

    .clear {
      clear: both;
    }

    .mt0 {
      margin-top: 0;
    }

    .mt20 {
      margin-top: 20px;
    }

    .mb0 {
      margin-bottom: 0;
    }

    .mb20 {
      margin-bottom: 20px;
    }

    .powered-by a {
      text-decoration: none;
    }

    .powered-by img {
      height: 20px;
    }

    hr {
      border: 0;
      border-bottom: 1px solid #F1F4F8;
      margin: 20px 0;
    }

    .salutation {
      color: #000000;
      font-weight: bold;
      font-size: 18px;
      margin-bottom: 15px;
    }

    .code {
      background: #F1F4F8;
      color: #000000;
      text-align: center;
      font-size: 30px;
      font-weight: bold;
      width: auto;
      padding: 10px;
      margin-top: 20px;
      margin-bottom: 5px;
      border-radius: 5px
    }

    .italic {
      font-style: italic;
    }

    /* -------------------------------------
          RESPONSIVE AND MOBILE FRIENDLY STYLES
      ------------------------------------- */
    @media only screen and (max-width: 620px) {
      table.body h1 {
        font-size: 22px !important;
        margin-bottom: 15px !important;
      }

      table.body p,
      table.body ul,
      table.body ol,
      table.body td,
      table.body span,
      table.body a {
        font-size: 16px !important;
      }

      table.body .wrapper,
      table.body .article {
        padding: 15px !important;
      }

      table.body .content {
        padding: 15px !important;
        margin: 0px !important;
      }

      table.body .container {
        padding: 0 !important;
        width: 100% !important;
      }

      table.body .main {
        border-left-width: 0 !important;
        border-radius: 0 !important;
        border-right-width: 0 !important;
        border-radius: 5px !important;
      }

      table.body .btn table {
        width: 100% !important;
      }


      table.body .img-responsive {
        height: auto !important;
        max-width: 100% !important;
        width: auto !important;
      }
    }

    /* -------------------------------------
          PRESERVE THESE STYLES IN THE HEAD
      ------------------------------------- */
    @media all {
      .ExternalClass {
        width: 100%;
      }

      .ExternalClass,
      .ExternalClass p,
      .ExternalClass span,
      .ExternalClass font,
      .ExternalClass td,
      .ExternalClass div {
        line-height: 100%;
      }

      .apple-link a {
        color: inherit !important;
        font-family: inherit !important;
        font-size: inherit !important;
        font-weight: inherit !important;
        line-height: inherit !important;
        text-decoration: none !important;
      }

      #MessageViewBody a {
        color: inherit;
        text-decoration: none;
        font-size: inherit;
        font-family: inherit;
        font-weight: inherit;
        line-height: inherit;
      }
    }
  </style>
</head>

<body>
  <table role="presentation" border="0" cellpadding="0" cellspacing="0" class="body">
    <tr>
      <td class="container">
        <div class="content">

          <!-- START CENTERED WHITE CONTAINER -->
          <table role="presentation" class="main">

            <!-- START MAIN CONTENT AREA -->
            <tr>
              <td class="wrapper">
                <table role="presentation" border="0" cellpadding="0" cellspacing="0">
                  <tr>
                    <td>
                      <a href="https://www.zaut.tech/">
                        <img height="24px"
                          src="https://cdn.discordapp.com/attachments/938388618479804478/993567353516339230/Logo.png">
                        </img>
                      </a>

                      <hr>

                      <h1>
                        Olá, você foi convidado para fazer parte do time da {{companyName}}
                      </h1>

                      <p>
                        Este é o seu código de ativação de conta. Para utiliza-lo clique no link a seguir
                        <a href="{{url}}">Ativar conta</a>
                      </p>

                      <div class="code">{{code}}</div>

                      <p>
                        <span class="italic">
                          * O código é válido por {{numberDaysToExpire}} dias
                        </span>
                      </p>

                      <p>
                        <strong>Não compartilhe este código com outras pessoas.</strong>
                      </p>

                      <p>
                        Por que estamos enviando esse email? Levamos sua segurança muito
                        a sério e vamos sempre manter
                        você informado(a) sobre movimentações importantes na sua conta
                        Zaut.
                      </p>

                      <p>
                        Atenciosamente, <br>
                        <strong>
                          Equipe Zaut
                        </strong>
                      </p>

                      <hr>

                      <span>
                        Se estiver com problemas para clicar no botão "Ativar conta", copie e cole a URL abaixo em seu
                        navegador
                        <a href="{{url}}">{{url}}</a>
                      </span>
                    </td>
                  </tr>
                </table>
              </td>
            </tr>

            <!-- END MAIN CONTENT AREA -->
          </table>
          <!-- END CENTERED WHITE CONTAINER -->

          <!-- START FOOTER -->
          <div class="footer">
            <table role="presentation" border="0" cellpadding="0" cellspacing="0">
              <tr>
                <td class="content-block powered-by">
                  <a href="https://www.zaut.tech/">
                    <img src="https://cdn.discordapp.com/attachments/938388618479804478/993595785847521290/Vector_1.png"/>
                    <div>&copy; 2022</div>
                  </a>
                </td>
              </tr>
            </table>
          </div>
          <!-- END FOOTER -->

        </div>
      </td>
    </tr>
  </table>
</body>

</html>
```
