# BlazorTicTacToe
<img src="Assets/BlazorWebAssemblyC.png" height=300>

## What is Blazor WebAssembly?
[Blazor WebAssembly](https://docs.microsoft.com/en-us/aspnet/core/blazor/?view=aspnetcore-3.1#:~:text=Blazor%20WebAssembly%20is%20a%20single-page%20app%20%28SPA%29%20framework,in%20all%20modern%20web%20browsers%2C%20including%20mobile%20browsers.) is a [single-page app framework](https://docs.microsoft.com/en-us/dotnet/architecture/modern-web-apps-azure/choose-between-traditional-web-and-single-page-apps) such as Angular, VueJS, and React, that allows one to write UI logic in C#. Running .Net code inside the browser without any plugins or recompiling code into other languages. Blazor WebAssembly includes a proper .NET runtime implemented in WebAssembly, a standardized byte-code for the web. This .NET runtime is downloaded with your Blazor WebAssembly app and enables running normal .NET code directly in the browser.

## Getting Started
A few things are neede to get started on Blazor Web Assembly Project
* Install the [.Net Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1)
* Install [Visual Studio Code](https://code.visualstudio.com/)
* Install the latest version of the [C# extension in VS Code](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp)

After installing the above tools, Clone Git repository from GitHub::
```
git clone https://github.com/JuansonGrajales/BlazorTicTacToe.git
```
Switch into the cloned folder:
```
cd BlazorTicTacToe
```
Once you've ran the command above, open the folder and run the following command from VS Code terminal:
```
dotnet run
```
Open the browser and navigate to http://localhost:5000 or https://localhost:5001. You should be able to see the application something like the following :

<img src="Assets/blazorIndex.jpg" height=300>

## Components
Blazor apps are built with [components](https://docs.microsoft.com/en-us/aspnet/core/blazor/components/?view=aspnetcore-3.1#:~:text=Blazor%20apps%20are%20built%20using%20components.%20A%20component,to%20UI%20events.%20Components%20are%20flexible%20and%20lightweight.). A component is a self-contained chunk of user interface, such as a page, dialog, or form. They include HTML markup and the processing logic required to inject data or respond to UI events. For this project, two components were build:

### Square.razor

```C#
<div class="square" @onclick="ClickHandler">@value</div>
@code{
    //---properties
    [Parameter]
    public char value { get; set; }
    [Parameter]
    public EventCallback ClickHandler { get; set; }
}
```

### Board.razor

```C#
@{
    var gameStatus = Helper.CalculateGameStatus(values);
    string status;
    if (gameStatus == GameStatus.X_wins)
    {
        status = "Winner: X";
    }
    else if (gameStatus == GameStatus.O_wins)
    {
        status = "Winner: O";
    }
    else if (gameStatus == GameStatus.Draw)
    {
        status = "Draw!";
    }
    else
    {
        char nextPlayer = xIsNext ? 'X' : 'O';
        status = $"Next player: {nextPlayer}";
    }
}
<h3>@status</h3>
<div class="board">
    @for (int i = 0; i < 9; i++)
    {
        int squareNumber = i;
        <Square @key=squareNumber value=values[squareNumber] ClickHandler="@(() => HandleClick(squareNumber))" />
    }
</div>

@code{
    //---fields
    private bool xIsNext;
    private char[] values = new char[9];

    //---methods
    protected override void OnInitialized()
    {
        InitState();
    }
    private void PlayAgainHandler()
    {
        InitState();
    }
    private void InitState()
    {
        values = new char[9]{
            ' ', ' ', ' ',
            ' ', ' ', ' ',
            ' ', ' ', ' '
        };
        xIsNext = true;
    }

    private void HandleClick(int i)
    {
        if (values[i] != ' ')
        {
            return;
        }
        bool isGameFinish = Helper.CalculateGameStatus(values) != GameStatus.NotYetFinished;
        if (isGameFinish)
        {
            return;
        }
        bool xToPlay = xIsNext;
        values[i] = xToPlay ? 'X' : 'O';
        xIsNext = !xToPlay;
    }
}
```

