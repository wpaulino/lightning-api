```python
>>> import codecs, grpc, os
>>> import rpc_pb2 as ln, rpc_pb2_grpc as lnrpc{% if method.service != 'WalletUnlocker' %}
>>> macaroon = codecs.encode(open('LND_DIR/data/chain/bitcoin/simnet/admin.macaroon', 'rb').read(), 'hex'){% endif %}
>>> os.environ['GRPC_SSL_CIPHER_SUITES'] = 'HIGH+ECDSA'
>>> cert = open('LND_DIR/tls.cert', 'rb').read()
>>> ssl_creds = grpc.ssl_channel_credentials(cert)
>>> channel = grpc.secure_channel('localhost:10009', ssl_creds)
>>> stub = lnrpc.{{ method.service }}Stub(channel){% if method.streamingRequest %}
{% include 'python/streaming_request.html' %}{% else %}
{% include 'python/simple_request.html' %}{% endif %}{% if method.streamingResponse %}
{% include 'python/streaming_response.html' %}{% else %}
{% include 'python/simple_response.html' %}{% endif %}
{ {% for param in method.responseMessage.params %}
    "{{ param.name }}": <{{ param.type }}>,{% endfor %}
}
```
