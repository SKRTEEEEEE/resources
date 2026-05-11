# Tutorial Extendido: Uso avanzado del componente toast() de shadcn/ui

## 1. Instalación y Configuración Inicial

Primero, asegúrate de tener instalado shadcn/ui en tu proyecto. Si aún no lo has hecho, puedes instalarlo con el siguiente comando:

```bash
npx shadcn-ui@latest add toast
```

Esto instalará los componentes necesarios y creará los archivos correspondientes en tu proyecto.

## 2. Importación y Uso Básico

```javascript
import { useToast } from "@/components/ui/use-toast"
import { Button } from "@/components/ui/button"

export function ToastDemo() {
  const { toast } = useToast()
  
  return (
    <Button
      onClick={() => {
        toast({
          title: "Toast básico",
          description: "Este es un toast simple",
        })
      }}
    >
      Mostrar Toast Básico
    </Button>
  )
}
```

## 3. Opciones Avanzadas de Configuración

### 3.1. Variantes y Estilos

shadcn/ui proporciona varias variantes predefinidas, pero también puedes crear las tuyas propias:

```javascript
const { toast } = useToast()

// Variante destructiva
toast({
  variant: "destructive",
  title: "Error crítico",
  description: "No se pudo completar la operación.",
})

// Variante personalizada (asumiendo que has definido una variante 'success' en tu CSS)
toast({
  variant: "success",
  title: "¡Operación exitosa!",
  description: "Los cambios se han guardado correctamente.",
})

// Estilo personalizado
toast({
  title: "Toast con estilo personalizado",
  description: "Este toast tiene un fondo diferente",
  style: {
    background: 'linear-gradient(to right, #00b09b, #96c93d)',
    color: 'white',
  },
})
```

### 3.2. Acciones y Componentes Personalizados

Puedes añadir acciones y componentes personalizados a tus toasts:

```javascript
import { ToastAction } from "@/components/ui/toast"
import { Badge } from "@/components/ui/badge"

toast({
  title: "Actualización disponible",
  description: "Una nueva versión está lista para instalar.",
  action: (
    <ToastAction altText="Actualizar ahora" onClick={() => console.log("Iniciando actualización")}>
      Actualizar
    </ToastAction>
  ),
})

toast({
  title: "Notificación importante",
  description: (
    <div>
      Nuevo mensaje recibido
      <Badge variant="secondary" className="ml-2">Nuevo</Badge>
    </div>
  ),
})
```

### 3.3. Duración y Comportamiento

```javascript
// Toast con duración personalizada
toast({
  title: "Autodesaparición rápida",
  description: "Este toast desaparecerá en 2 segundos",
  duration: 2000,
})

// Toast persistente
toast({
  title: "Notificación importante",
  description: "Este toast no desaparecerá automáticamente",
  duration: Infinity,
})
```

### 3.4. Posicionamiento Avanzado

Puedes controlar la posición de los toasts modificando el componente `Toaster` en tu layout:

```javascript
import { Toaster } from "@/components/ui/toaster"

export function Layout({ children }) {
  return (
    <>
      {children}
      <Toaster position="top-center" />
    </>
  )
}
```

Las opciones de posición incluyen: `top-left`, `top-center`, `top-right`, `bottom-left`, `bottom-center`, `bottom-right`.

### 3.5. Manejo de Eventos

```javascript
toast({
  title: "Toast interactivo",
  description: "Este toast tiene eventos personalizados",
  onOpenChange: (open) => {
    if (!open) {
      console.log("El toast se ha cerrado");
    }
  },
  onSwipeStart: (event) => {
    console.log("El usuario comenzó a deslizar el toast");
  },
  onSwipeMove: (event) => {
    console.log("El usuario está deslizando el toast");
  },
  onSwipeEnd: (event) => {
    console.log("El usuario terminó de deslizar el toast");
  },
})
```

### 3.6. Toasts con Contenido Rico

Puedes incluir contenido más rico en tus toasts, como imágenes o componentes personalizados:

```javascript
import { Avatar } from "@/components/ui/avatar"

toast({
  title: "Nuevo seguidor",
  description: (
    <div className="flex items-center">
      <Avatar className="mr-2">
        <img src="/avatar.png" alt="Avatar" />
      </Avatar>
      <span>Juan Pérez ahora te sigue</span>
    </div>
  ),
})
```

### 3.7. Toasts Anidados o en Secuencia

Puedes mostrar múltiples toasts en secuencia o incluso anidarlos:

```javascript
const showSequentialToasts = () => {
  toast({
    title: "Paso 1",
    description: "Iniciando proceso...",
  })

  setTimeout(() => {
    toast({
      title: "Paso 2",
      description: "Procesando datos...",
    })
  }, 2000)

  setTimeout(() => {
    toast({
      title: "Paso 3",
      description: "¡Proceso completado!",
      variant: "success",
    })
  }, 4000)
}
```

### 3.8. Toasts con Progreso

Aunque shadcn/ui no tiene una barra de progreso incorporada para los toasts, puedes crear una personalizada:

```javascript
import { useState, useEffect } from 'react'
import { Progress } from "@/components/ui/progress"

const ProgressToast = () => {
  const [progress, setProgress] = useState(0)
  const { toast } = useToast()

  useEffect(() => {
    const timer = setInterval(() => {
      setProgress((prevProgress) => {
        if (prevProgress >= 100) {
          clearInterval(timer)
          return 100
        }
        return prevProgress + 10
      })
    }, 1000)

    return () => clearInterval(timer)
  }, [])

  useEffect(() => {
    if (progress === 100) {
      toast({
        title: "¡Proceso completado!",
        description: "Todos los datos han sido procesados.",
        variant: "success",
      })
    }
  }, [progress, toast])

  return (
    <div>
      <h3>Procesando datos...</h3>
      <Progress value={progress} className="w-[100%]" />
    </div>
  )
}

// Uso
toast({
  title: "Procesamiento en curso",
  description: <ProgressToast />,
  duration: 10000,
})
```

## 4. Mejores Prácticas y Consideraciones

1. **Accesibilidad**: Asegúrate de que la información crítica no se transmita únicamente a través de toasts. Proporciona alternativas para usuarios que dependen de lectores de pantalla.

2. **No sobrecargues**: Usa los toasts con moderación. Demasiadas notificaciones pueden abrumar al usuario.

3. **Consistencia**: Mantén un estilo y comportamiento consistente en todos tus toasts para una mejor experiencia de usuario.

4. **Pruebas**: Asegúrate de probar tus toasts en diferentes dispositivos y tamaños de pantalla para garantizar una buena experiencia en todos los casos.

5. **Personalización**: Aprovecha la flexibilidad de shadcn/ui para adaptar los toasts a la identidad visual de tu aplicación.

# Tutorial Extendido: Uso avanzado del componente toast() de shadcn/ui - Parte 2

## 5. Casos de Uso Avanzados

### 5.1. Toast con Temporizador

Puedes crear un toast que muestre un temporizador de cuenta regresiva:

```javascript
import React, { useState, useEffect } from 'react'
import { useToast } from "@/components/ui/use-toast"

function CountdownToast({ duration }) {
  const [timeLeft, setTimeLeft] = useState(duration / 1000)
  const { toast } = useToast()

  useEffect(() => {
    const timer = setInterval(() => {
      setTimeLeft((prevTime) => {
        if (prevTime <= 1) {
          clearInterval(timer)
          return 0
        }
        return prevTime - 1
      })
    }, 1000)

    return () => clearInterval(timer)
  }, [])

  return (
    <div>
      <p>Tiempo restante: {timeLeft} segundos</p>
    </div>
  )
}

// Uso
toast({
  title: "Oferta por tiempo limitado",
  description: <CountdownToast duration={60000} />,
  duration: 60000,
})
```

### 5.2. Toast con Formulario Interactivo

Puedes incluir un pequeño formulario dentro de un toast para una interacción rápida:

```javascript
import React, { useState } from 'react'
import { useToast } from "@/components/ui/use-toast"
import { Input } from "@/components/ui/input"
import { Button } from "@/components/ui/button"

function SubscribeToast() {
  const [email, setEmail] = useState('')
  const { toast } = useToast()

  const handleSubmit = (e) => {
    e.preventDefault()
    // Lógica para manejar la suscripción
    toast({
      title: "¡Gracias por suscribirte!",
      description: `Te hemos enviado un correo a ${email}`,
    })
  }

  return (
    <form onSubmit={handleSubmit}>
      <Input
        type="email"
        placeholder="Tu correo electrónico"
        value={email}
        onChange={(e) => setEmail(e.target.value)}
        className="mb-2"
      />
      <Button type="submit">Suscribirse</Button>
    </form>
  )
}

// Uso
toast({
  title: "¡Suscríbete a nuestro boletín!",
  description: <SubscribeToast />,
  duration: Infinity,
})
```

### 5.3. Toast con Confirmación

Puedes usar un toast para solicitar una confirmación rápida al usuario:

```javascript
import { useToast } from "@/components/ui/use-toast"
import { ToastAction } from "@/components/ui/toast"

function ConfirmationToast() {
  const { toast } = useToast()

  const handleConfirm = () => {
    // Lógica para manejar la confirmación
    toast({
      title: "Acción confirmada",
      description: "Has confirmado la acción exitosamente.",
    })
  }

  const handleCancel = () => {
    // Lógica para manejar la cancelación
    toast({
      title: "Acción cancelada",
      description: "Has cancelado la acción.",
      variant: "destructive",
    })
  }

  return (
    <div>
      <p>¿Estás seguro de que quieres realizar esta acción?</p>
      <div className="mt-2">
        <ToastAction altText="Confirmar" onClick={handleConfirm}>Confirmar</ToastAction>
        <ToastAction altText="Cancelar" onClick={handleCancel}>Cancelar</ToastAction>
      </div>
    </div>
  )
}

// Uso
toast({
  title: "Confirmación requerida",
  description: <ConfirmationToast />,
  duration: Infinity,
})
```

## 6. Props Detalladas del Componente Toast

El componente toast() acepta un objeto de configuración con varias props. Aquí tienes una lista detallada de las props disponibles y cómo usarlas:

### 6.1. Props Básicas

- `title`: (string) El título principal del toast.
- `description`: (string | React.ReactNode) La descripción o contenido principal del toast.
- `duration`: (number) Duración en milisegundos que el toast permanecerá visible. Use `Infinity` para un toast persistente.

```javascript
toast({
  title: "Notificación importante",
  description: "Este es un mensaje crucial que permanecerá visible.",
  duration: Infinity,
})
```

### 6.2. Props de Estilo

- `variant`: (string) Define el estilo visual del toast. Valores predefinidos: `"default"`, `"destructive"`.
- `className`: (string) Clases CSS adicionales para personalizar el estilo del toast.
- `style`: (React.CSSProperties) Estilos en línea para el toast.

```javascript
toast({
  title: "Error de sistema",
  description: "Se ha producido un error crítico.",
  variant: "destructive",
  className: "font-bold",
  style: { borderWidth: '2px', borderColor: 'red' },
})
```

### 6.3. Props de Acción

- `action`: (React.ReactNode) Un componente de acción para el toast, típicamente un botón.
- `cancel`: (React.ReactNode) Un componente para cancelar o cerrar el toast.

```javascript
toast({
  title: "Actualización disponible",
  description: "Una nueva versión está lista para instalar.",
  action: <ToastAction altText="Actualizar ahora">Actualizar</ToastAction>,
  cancel: <ToastAction altText="Cancelar" variant="secondary">Cancelar</ToastAction>,
})
```

### 6.4. Props de Evento

- `onOpenChange`: (open: boolean) => void) Se llama cuando el estado de apertura del toast cambia.
- `onSwipeStart`: (event: SwipeEvent) => void) Se llama cuando comienza un gesto de deslizamiento.
- `onSwipeMove`: (event: SwipeEvent) => void) Se llama durante un gesto de deslizamiento.
- `onSwipeEnd`: (event: SwipeEvent) => void) Se llama cuando termina un gesto de deslizamiento.

```javascript
toast({
  title: "Toast interactivo",
  description: "Observa la consola para ver los eventos",
  onOpenChange: (open) => console.log(`Toast ${open ? 'abierto' : 'cerrado'}`),
  onSwipeStart: () => console.log("Inicio de deslizamiento"),
  onSwipeMove: () => console.log("Deslizamiento en progreso"),
  onSwipeEnd: () => console.log("Fin de deslizamiento"),
})
```

### 6.5. Props Avanzadas

- `id`: (string) Un identificador único para el toast. Útil para actualizar o cerrar toasts específicos.
- `asChild`: (boolean) Si es true, el toast se renderizará como su hijo directo en lugar de envolver el contenido.

```javascript
const toastId = 'unique-toast-id'

toast({
  id: toastId,
  title: "Toast actualizable",
  description: "Este toast puede ser actualizado más tarde",
})

// Más tarde en el código...
toast({
  id: toastId,
  title: "Toast actualizado",
  description: "El contenido ha sido actualizado",
})
```

## 7. Personalización Avanzada del Componente Toaster

El componente `Toaster` que normalmente se coloca en el layout de tu aplicación también acepta props para personalización:

```javascript
import { Toaster } from "@/components/ui/toaster"

<Toaster 
  position="top-right"
  reverseOrder={false}
  toastOptions={{
    duration: 5000,
    style: {
      background: '#363636',
      color: '#fff',
    },
  }}
/>
```

- `position`: Define la posición por defecto de los toasts.
- `reverseOrder`: Si es true, los nuevos toasts se añaden al principio en lugar del final.
- `toastOptions`: Opciones por defecto para todos los toasts.

Recuerda que las opciones específicas de cada toast sobrescribirán estas opciones globales.