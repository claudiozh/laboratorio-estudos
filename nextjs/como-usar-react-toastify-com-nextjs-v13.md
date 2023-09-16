# 🙉 Como usar React Toastify com NextJS V13

Criar provider que utilize o ToastContainer da lib React Toastify

{% code title="src/providers/toast.provider.tsx" %}
```tsx
"use client";

import { ToastContainer } from "react-toastify";
import 'react-toastify/dist/ReactToastify.css';

interface ToastProviderProps {
  children: React.ReactNode;
}

export default function ToastProvider({ children }: ToastProviderProps) {
  return (
    <>
      {children}
      <ToastContainer />
    </>
  );
}
```
{% endcode %}

Usar o provider ToastProvider no layout da aplicação para ficar disponível em todas páginas

```tsx
import ToastProvider from "@/providers/toast.provider";

export default async function RootLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
      <html lang="en">
        <body suppressHydrationWarning>
          <ToastProvider>
            {children}
          </ToastProvider>
        </body>
      </html>
    </AuthProvider>
  );
}
```
