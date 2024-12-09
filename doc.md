# Un peu de doc

## Dernières nouveautés

https://react.dev/blog/2024/12/05/react-19
https://react-hook-form.com/
https://vike.dev/
https://tanstack.com/
https://zustand.docs.pmnd.rs/getting-started/introduction

## Hooks (patterns)

-> Utiliser le hook comme unique interface qui des services et confs

```jsx
    import {
        VIEW_TRUNCATE,
        VIEW_TRUNCATE_MORE,
        VIEW_TRUNCATE_LESS,
        VIEW_TRUNCATE_LENGTH,
    } from '@/configs';
    import { formaterService } from '@/services';

    export const useFormater = () => ({
        toUpperCase: (string: string): string => formaterService().uppercase(string),
        toCapitalize: (string: string): string => formaterService().capitalize(string),
        toTruncate: (string: string): string => formaterService().truncate(string, VIEW_TRUNCATE_LENGTH, VIEW_TRUNCATE),
        toTruncateLess: (string: string): string => formaterService().truncate(string, VIEW_TRUNCATE_LENGTH, VIEW_TRUNCATE_LESS),
        toTruncateMore: (string: string): string => formaterService().truncate(string, VIEW_TRUNCATE_LENGTH, VIEW_TRUNCATE_MORE),
        toAdd: (numbers: number[]): number => formaterService().add(numbers),
    });
```

Utiliser des hooks as wraps pour ajouter des fonctionnalités(comme des décorateurs):

Page.tsx

```jsx
        <Log
            type={REGISTER_LOG_LOADING.type}
            message={REGISTER_LOG_LOADING.message}
        >
            <Form submit={handleSubmit} error={''} />
        </Log>

```

```jsx
import { LOGGER_PATH } from "@/configs";
import { loggerService } from "@/services/logger/logger.service";
import { ReactNode } from 'react';

interface ILog {
    type: 'info' | 'error' | 'warn',
    message: string,
    children: ReactNode
}

export const useLog = () => {
    const logger = loggerService().transport(LOGGER_PATH);

    const info = (message: string) => logger.info(message);
    const warn = (message: string) => logger.warn(message);
    const error = (message: string) => logger.error(message);


    const Log = (props: ILog) => {
        const { type, message, children } = props;

        switch (type) {
            case 'info':
                info(message);
                break;
            case 'error':
                error(message);
                break;
            case 'warn':
                warn(message);
                break;
        }
        return (
            <>
                {children}
            </>
        )
    }

    return {
        info,
        error,
        warn,
        Log,
    }
}
```

## Update

-> didmount

```jsx
useEffect(() => {
  // Your code here
}, []);
```

-> didupdate

```jsx
useEffect(() => {
  // Your code here
}, [yourDependency]);
```

willunmount

```jsx
useEffect(() => {
  // componentWillUnmount
  return () => {
     // Your code here
  }
}, [yourDependency]);
```
