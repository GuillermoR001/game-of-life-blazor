# In honor to John Conway.
---

Game of life using blazor and C# ![Blazor](/GameOfLife/Client/wwwroot/favicon.png)

Feel free to use it if you want :)

Next, a couple of images to see the process I followed to build this one. It need to improve a lot but yeah, I'm proud of it.

1 - Create the initial grid.
```cs
    const int ROWS = 24;
    const int COLS = 24;
    public bool isRunning { get; set; } = false;
    public int generation { get; set; } = 0;
    private int[,] gridCopy = new int[ROWS, COLS];
    private int[,] grid = new int[ROWS, COLS];
```
```cs
    <table class="gof-table">
        <tbody>
            @for (int i = 0; i < grid.GetLength(0); i++)
            {
                <tr>
                    @for (int j = 0; j < grid.GetLength(1); j++)
                    {
                        int row = i;
                        int col = j;
                        var cell = grid[row, col];
                        <td class="cell @(cell == 1 ? " bg-primary" : null)" onclick="@(() => ActiveCell(row,col, cell))">&nbsp;</td>
                    }
                </tr>
            }
        </tbody>
    </table>
```
![Grid](/GameOfLife/Client/wwwroot/images/gof-1.PNG)

```cs
    /// <summary>
    /// Activer o deactive cell iside the board
    /// </summary>
    /// <param name="row"></param>
    /// <param name="col"></param>
    /// <param name="currenState"></param>
    private void ActiveCell(int row, int col, int currenState)
    {
        if ((row >= 0 && row <= ROWS) && (col >= 0 && col <= ROWS))
        {
            grid[row, col] = currenState == 1 ? 0 : 1;
            StateHasChanged();
        }
    }
```
2 - Add the state live and death to the cells.
![State](/GameOfLife/Client/wwwroot/images/gof-2.PNG)

```cs
        isRunning = true;
        gridCopy = (int[,])grid.Clone();
        while (isRunning)
        {
            for (int row = 1; row < gridCopy.GetLength(0) - 1; row++)
            {
                for (int col = 1; col < gridCopy.GetLength(1) - 1; col++)
                {
                    int neighbors = CountNeighbors(row, col);
                    if (gridCopy[row, col] == 0 && neighbors == 3)
                    {
                        gridCopy[row, col] = 1;
                    }
                    else if (gridCopy[row, col] == 1 && (neighbors == 2 || neighbors == 3))
                    {
                        gridCopy[row, col] = 1;
                    }
                    else if (gridCopy[row, col] == 1 && (neighbors < 2 || neighbors > 3))
                    {
                        gridCopy[row, col] = 0;
                    }
                }
            }
            generation += 1;
            grid = (int[,])gridCopy.Clone();
```
3 - Iterate generations and add some controls
![Iterate](/GameOfLife/Client/wwwroot/images/gof-3.PNG)

4 - Add some style and refactor some code ;)
![Iterate](/GameOfLife/Client/wwwroot/images/gof-4.PNG)

