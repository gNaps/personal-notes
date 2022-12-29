# Prisma ORM

Created: December 24, 2021
Status: In Progress
Type: Backend

# Start with prisma

`npx prisma init` 
Una volta costruito il Database importare nel proprio progetto prisma con yarn. Utilizzare quindi la cli di prisma tramite i comandi npx. 
`npx prisma db pull` 
permette di allineare il nostro file di schema con il nostro DB (ho dovuto cancellarlo ogni volta prima di rilanciare altrimenti andava in errore).
Mantenere sempre i nomi dei modelli e dei campi con iniziali minuscoli, inoltre è possibile cambiare a piacimento i nomi delle relazione poiché non esistono nel DB.
Se si desidera cambiare il nome di un campo presente nel DB si può utilizzare il mapping di prisma
`userId INT @id @map("user_id")` 
Una volta costruiti i model si può procedere con l’installazione e la generazione di Prisma Client. Installare @prisma/client con yarn ed eseguire
`npx prisma generate`

# Querying the database

### First query

1. Importa `PrismaClient` constructor da `@prisma/client` node module
2. Instanzia `PrismaClient`
3. Definisci una `async` function per eseguire la query
4. Chiudi la connessione al DB

Si utilizza il metodo findMany per recuperare tutti i records nel DB

### Write into DB

Per scrivere un nuovo record sul db usare la funzione create: prisma.MODEL_NAME.create({})
Per l’update prima.user.update({})
In caso di relazioni basta indicare nel campo data anche gli attributi di riferimento creati dallo schema.prisma

### Update DB from prisma

Per aggiornare il DB a partire dal codice (quindi dallo schema.prisma) è possibile utilizzare l’apposito comando 
`npx prisma db push`