# volopay-einvoicing-prototype
Volopay E-Invoicing &amp; AR Prototype Architecture
[ Volopay Web Frontend (React / SPA) ]
            │  (HTTPS / REST JSON Payload)
            ▼
[ Volopay Backend API Gateway & Validation Engine ]
            │  (Schema & Compliance Check)
    ┌───────┴────────────────────────┐
    ▼                                ▼
[ PostgreSQL DB ]          [ E-Invoicing Partner API ]
 (Local App State)          (ClearTax / Storecove / Peppol Access Point)
    │                                │
    │ (Webhook Status Update)        ▼
    └──────────────► [ Government Tax Portal / IRP / IRAS ]
                     (IRN Generation / Digital Signature)
