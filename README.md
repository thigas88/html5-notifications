# Notificações HTML5 em tempo real

Demostração de como utilizar Elasticpush para gerar notificações HTML5 em seu site.

### Exemplo de chamada pelo backend
<pre >
<code class="bash" >
curl -HContent-Type:application/json \
-HX-Token:15f80686c2cd6a75f1eac18de966e35dd35cf649ac730999e0bc34bc1e013b5e \
-HX-Secret-Token:197fbd50835cfb0ccb892e03b1af3c496a87cbc26cdc864a2f1ba50b7a2c46c1 \
-d '{"channel": "elasticpush-test","identifier":"<span id="clientId"></span>","event":"event-test","data":{"message":"Hello world!"}}' \
"https://api.elasticpush.com/v1/apps/63/events"
</code>
</pre>

###  No frontend
<pre><code>
if(!Notification){
    alert('Seu browser não suporta notificações!');
}else{
    getPermission();
}
/**
 * Pergunta pro usuário se ele permite o seu app usar as notificações
 */
function getPermission() {
    if (Notification.permission != 'granted') {
        Notification.requestPermission(function (permission) {
            if (permission === 'granted') {
            } else {
                alert('Você não permitiu o uso das notificações');
            }
        });
    } 
}

var client_id = new Date().getTime();
document.getElementById("clientId").innerHTML = client_id;
var elasticpush = new Elasticpush('15f80686c2cd6a75f1eac18de966e35dd35cf649ac730999e0bc34bc1e013b5e');
var app = elasticpush.subscribe('elasticpush-test');

/**
 * ID do usuário no seu sistema
 */
app.setClientId(client_id);

app.on('event-test', function(data){
    if(Notification){
        //Gera uma notificação a partir de um evento disparado pelo backend
        var notification = new Notification("Notification test", {
            body: data.message,
            icon:'https://avatars0.githubusercontent.com/u/12091630?v=3&s=200'
        });

        //Fecha a notificação em 4 segundos
        setTimeout(function(){
            notification.close();
        }, 4000);
    }else{
        //Usa alert caso não tenha suporte para Notification
        alert(data.message);
    }
});
</code></pre>