# BlazorWebApp MicrosorAI Sample1

## Create a new razor component

```razor
@page "/chat"

@inject IJSRuntime JSRuntime

<h3>Chat with AI</h3>

<div class="mb-3">
    <label for="userInput" class="form-label">Your Question:</label>
    <input type="text" id="userInput" @bind="userInputText" class="form-control" />
</div>

<button class="btn btn-primary" @onclick="SendMessage">Send</button>

<div class="mt-4">
    <h5>Response:</h5>
    <div class="border p-3 rounded" style="min-height: 100px;">
        @if (!string.IsNullOrEmpty(aiResponse))
        {
            @aiResponse
        }
        else
        {
            <span class="text-muted">No response yet.</span>
        }
    </div>
</div>

@code {
    private string userInputText = string.Empty;
    private string aiResponse = string.Empty;

    private async Task SendMessage()
    {
        if (!string.IsNullOrWhiteSpace(userInputText))
        {
            aiResponse = await ChatWithAI(userInputText);
        }
    }

    private async Task<string> ChatWithAI(string userMessage)
    {
        var endpoint = new Uri("https://models.inference.ai.azure.com");
        var modelId = "gpt-4o-mini";
        var credential = new AzureKeyCredential(Environment.GetEnvironmentVariable("GH_TOKEN"));

        IChatClient client =
            new ChatCompletionsClient(endpoint, credential)
                .AsChatClient(modelId);

        var response = await client.CompleteAsync(userInputText);

        return response.Message.ToString();
    }
}
```

## Run the application and see the result

![image](https://github.com/user-attachments/assets/c8a6ec3e-713d-4671-ac82-c0bce6c99860)


