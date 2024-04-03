# React Material UI v5

[v5 Installation Docs](https://mui.com/material-ui/getting-started/installation/)

```
npm install @mui/material@5 @emotion/react @emotion/styled 
npm install @fontsource/roboto
```

In App.jsx
`import '@fontsource/roboto';`

The Material UI docs can be overwhelming. Make sure you're using the correct version of the docs  -- we're using v5. v4 is NOT compatible with React 18.

Be patient with them and yourself. Don't assume you need all the things. Start small and work through adding things slowly. 



## Make a button

```JSX
// SearchButton.js
import React from 'react';
import Button from '@mui/material/Button';

function SearchButton () {
    
  return (
    <Button variant="raised" color="primary">
      Search
    </Button>)
    
}

export default SearchButton;
```

### One-Off Style in JavaScript

There is a `sx` prop. The sx prop is the best option for adding style overrides to a single instance of a component in most cases. It can be used with all Material UI components.

```JSX
import * as React from 'react';
import Box from '@mui/material/Box';

export default function BoxSx() {
  return (
    <Box
      sx={{
        width: 300,
        height: 300,
        backgroundColor: 'primary.dark',
        '&:hover': {
          backgroundColor: 'primary.main',
          opacity: [0.9, 0.8, 0.7],
        },
      }}
    />
  );
}
```



You can also use `styled` function. This might be better if you have reusable components.

Import and define styles above the Component:

```JSX
import React from 'react';
import { styled } from '@mui/material/styles';
import Button from '@mui/material/Button';
import { purple } from '@mui/material/colors';


const ColorButton = styled(Button)(({ theme }) => ({
  color: theme.palette.getContrastText(purple[500]),
  backgroundColor: purple[500],
  '&:hover': {
    backgroundColor: purple[700],
  },
}));

function CustomButton() {
  return (
      <ColorButton variant="contained">Custom CSS</ColorButton>
    );
}
```




## Add a theme

Themes are great for affecting all MUI components base styles.

[mui-theme-creator
](https://zenoo.github.io/mui-theme-creator/)


```JSX

import { createTheme, ThemeProvider } from '@mui/material/styles';

const themeOptions = {
  palette: {
    mode: 'dark',
    primary: {
      main: '#226d17',
    },
    secondary: {
      main: '#e600f5',
    },
  },
};

const theme = createTheme(themeOptions);

function App () {

    return (
      <ThemeProvider theme={theme}>
        <SearchButton />
      </ThemeProvider>
    );
  
}

export default App;
```

## Add a font

Install it

```
npm install fontsource-roboto
```

Then, you can import it in your entry-point.

```
import 'fontsource-roboto';
```

this won't apply to everything, just the Material-UI things

## Add an icon

```
npm install @mui/icons-material
```

```JSX
import { Search, Call } from '@mui/icons-material';
```

then, where you want the icon to go in the HTML, add

```JSX
<Search color="primary"/>
<Call color="secondary"/>
```

Each icon also has a "theme": Filled (default), Outlined, Rounded, Two tone and Sharp. If you want to import the icon component with a theme other than default, append the theme name to the icon name. For example @material-ui/icons/Delete icon with:

Filled theme (default) is exported as `@mui/icons-material/Delete`,
Outlined theme is exported as `@mui/icons-material/DeleteOutlined`,
Rounded theme is exported as `@mui/icons-material/DeleteRounded`,
Twotone theme is exported as `@material-ui/icons/DeleteTwoTone`,
Sharp theme is exported as `@material-ui/icons/DeleteSharp`.


Note: The Material Design specification names the icons using "snake_case" naming (for example delete_forever, add_a_photo), while @material-ui/icons exports the respective icons using "PascalCase" naming (for example DeleteForever, AddAPhoto). There are three exceptions to this naming rule: 3d_rotation exported as ThreeDRotation, 4k exported as FourK, and 360 exported as ThreeSixty.
