# Solución a preguntas del reto

1. ¿Qué habría que hacer para lanzar una campaña en USDC en vez de XLM?

Al momento de desplegar el contrato (por ejemplo, usando el SDK de Stellar) con el ID del contrato, al llamar la función `initialize`, usar la dirección del asset de USDC de Stellar, por ejemplo `USDC:GBBD47IF6LWK7P7MDEVSCWR7DPUWV3NY3DTQEVFL4NAT4AQH3ZLLFLA5`, en lugar de usar el nativo de Stellar

2. ¿Por qué un asistente con una wallet recién creada no podría contribuir a esa campaña? ¿Qué le falta a su cuenta?

Para poder crear una wallet, se necesita añadir fondos. Cuando una persona crea una Wallet usando, por ejemplo, Freighter, esta parece usar Friendbot de Stellar para fondearla en Testnet al inicio, pero ésta aún no está "activa" para Mainnet. Freighter pide fondearla con al menos 2XLM para ello, por lo que en primera instancia y solamente habiendo creado la Wallet, realmente no se podría contribuir porque la wallet no está creada realmente en Mainnet.

3. ¿Qué le agregarías al contrato o al frontend para que esa persona no se quede trabada?

En el contrato, usaría la función [`exists`](https://docs.rs/soroban-sdk/latest/soroban_sdk/struct.Address.html#method.exists) de Soroban en la función de `contribute` para validar que la dirección de la Wallet exista; si no existe, lanzaría un error, habiéndolo antes añadido al Enum como `NoExistingUser = 12`. Del lado del frontend, al atrapar este error al intentar realizar la donación, mostraria un mensaje al usuario diciendo que la cuenta actual no está inicializada y sugiriéndole que fondee su cuenta para poder activarla. Sin embargo, es probable que él ya se haya dado cuenta de esto al intentar realizar la transacción y que su app de wallet se lo muestre.
