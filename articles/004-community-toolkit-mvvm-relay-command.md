---
title: "WinUI x CommunityToolkit.Mvvm [RelayCommand編]"
emoji: "🛠️"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["WinUI", "WinUI3", "WindowsAppSDK", "XAML", "MVVM"]
published: true
---

[前回の記事](https://zenn.dev/andrewkeepcodin/articles/003-community-toolkit-mvvm-observable-property)では、
- `ObservableObject`
- `ObservableProperty`
を紹介しましたが、この記事では`RelayCommand`のサンプルコードを紹介します。

# UI
![](/images/004/firstname-lastname-fullname-list.gif)

# サンプルコード

```cs:SomePageViewModel.cs
using CommunityToolkit.Mvvm.ComponentModel;
using CommunityToolkit.Mvvm.Input;
using System.Collections.ObjectModel;

namespace CommunityToolkitMvvmDemo;

public record Person(string FirstName, string LastName);

public partial class SomePageViewModel : ObservableObject
{
    [ObservableProperty]
    [NotifyPropertyChangedFor(nameof(FullName))]
    [NotifyCanExecuteChangedFor(nameof(AddPersonToListCommand))]
    private string _firstName = string.Empty;

    [ObservableProperty]
    [NotifyPropertyChangedFor(nameof(FullName))]
    [NotifyCanExecuteChangedFor(nameof(AddPersonToListCommand))]
    private string _lastName = string.Empty;

    [ObservableProperty]
    private ObservableCollection<Person> _people = [];

    public string FullName => $"{FirstName} {LastName}";

    private bool CanAddPerson =>
        string.IsNullOrEmpty(FirstName) is false &&
        string.IsNullOrEmpty(LastName) is false;

    [RelayCommand(CanExecute = nameof(CanAddPerson))]
    private void AddPersonToList()
    {
        var person = new Person(FirstName, LastName);
        People.Add(person);
        FirstName = string.Empty;
        LastName = string.Empty;
    }

    [RelayCommand]
    private void RemoveFromList(Person person)
    {
        People.Remove(person);
    }
}
```

```cs:SomePage.xaml.cs
using Microsoft.UI.Xaml.Controls;

namespace CommunityToolkitMvvmDemo;

public sealed partial class SomePage : Page
{
    public SomePage()
    {
        InitializeComponent();
    }

    public SomePageViewModel ViewModel { get; } = new();
}
```

```xml:SomePage.xaml
<?xml version="1.0" encoding="utf-8" ?>
<Page
    x:Class="CommunityToolkitMvvmDemo.SomePage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:local="using:CommunityToolkitMvvmDemo"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    Background="{ThemeResource ApplicationPageBackgroundThemeBrush}"
    mc:Ignorable="d">

    <Page.Resources>
        <x:Double x:Key="Spacing">12</x:Double>
        <Style
            BasedOn="{StaticResource DefaultTextBoxStyle}"
            TargetType="TextBox">
            <Setter Property="Width" Value="200" />
        </Style>
    </Page.Resources>

    <Grid
        Padding="24"
        RowDefinitions="Auto,*"
        RowSpacing="{StaticResource Spacing}">
        <StackPanel
            Grid.Row="0"
            Spacing="{StaticResource Spacing}">
            <StackPanel
                Orientation="Horizontal"
                Spacing="{StaticResource Spacing}">
                <TextBox
                    Header="First Name"
                    Text="{x:Bind ViewModel.FirstName, Mode=TwoWay, UpdateSourceTrigger=PropertyChanged}" />
                <TextBox
                    Header="Last Name"
                    Text="{x:Bind ViewModel.LastName, Mode=TwoWay, UpdateSourceTrigger=PropertyChanged}" />
            </StackPanel>
            <StackPanel>
                <TextBlock Text="Full Name" />
                <TextBlock Text="{x:Bind ViewModel.FullName, Mode=OneWay}" />
            </StackPanel>
            <Button
                Command="{x:Bind ViewModel.AddPersonToListCommand}"
                Content="追加" />
            <Button
                Command="{x:Bind ViewModel.RemoveFromListCommand}"
                CommandParameter="{x:Bind PeopleList.SelectedItem, Mode=OneWay}"
                Content="削除"/>
        </StackPanel>

        <ListView
            x:Name="PeopleList"
            
            Grid.Row="1"
            ItemsSource="{x:Bind ViewModel.People}">
            <ListView.ItemTemplate>
                <DataTemplate x:DataType="local:Person">
                    <TextBlock>
                        <Run Text="{x:Bind FirstName}" />
                        <Run Text=" " />
                        <Run Text="{x:Bind LastName}" />
                    </TextBlock>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </Grid>

</Page>
```

# 解説

## `RelayCommand`属性

次のように、`AddPersonToList`メソッドに`RelayCommand`属性を付けると、

```cs
[RelayCommand]
private void AddPersonToList()
{
    var person = new Person(FirstName, LastName);
    People.Add(person);
    FirstName = string.Empty;
    LastName = string.Empty;
}
```

`CommunityToolkit.Mvvm`のソースジェネレーターがコマンド`AddPersonToListCommand`を自動生成してくれますので、

```cs
/// <summary>The backing field for <see cref="AddPersonToListCommand"/>.</summary>
[global::System.CodeDom.Compiler.GeneratedCode("CommunityToolkit.Mvvm.SourceGenerators.RelayCommandGenerator", "8.2.0.0")]
private global::CommunityToolkit.Mvvm.Input.RelayCommand? addPersonToListCommand;
/// <summary>Gets an <see cref="global::CommunityToolkit.Mvvm.Input.IRelayCommand"/> instance wrapping <see cref="AddPersonToList"/>.</summary>
[global::System.CodeDom.Compiler.GeneratedCode("CommunityToolkit.Mvvm.SourceGenerators.RelayCommandGenerator", "8.2.0.0")]
[global::System.Diagnostics.CodeAnalysis.ExcludeFromCodeCoverage]
public global::CommunityToolkit.Mvvm.Input.IRelayCommand AddPersonToListCommand => addPersonToListCommand ??= new global::CommunityToolkit.Mvvm.Input.RelayCommand(new global::System.Action(AddPersonToList));
```

XAMLで`Command`にバインディングできます。

```xml
<Button
    Command="{x:Bind ViewModel.AddPersonToListCommand}"
    Content="追加" />
```


もちろん引数にも対応しています。

```cs
[RelayCommand]
private void RemoveFromList(Person person)
{
    People.Remove(person);
}
```

```cs
/// <summary>The backing field for <see cref="RemoveFromListCommand"/>.</summary>
[global::System.CodeDom.Compiler.GeneratedCode("CommunityToolkit.Mvvm.SourceGenerators.RelayCommandGenerator", "8.2.0.0")]
private global::CommunityToolkit.Mvvm.Input.RelayCommand<global::CommunityToolkitMvvmDemo.Person>? removeFromListCommand;
/// <summary>Gets an <see cref="global::CommunityToolkit.Mvvm.Input.IRelayCommand{T}"/> instance wrapping <see cref="RemoveFromList"/>.</summary>
[global::System.CodeDom.Compiler.GeneratedCode("CommunityToolkit.Mvvm.SourceGenerators.RelayCommandGenerator", "8.2.0.0")]
[global::System.Diagnostics.CodeAnalysis.ExcludeFromCodeCoverage]
public global::CommunityToolkit.Mvvm.Input.IRelayCommand<global::CommunityToolkitMvvmDemo.Person> RemoveFromListCommand => removeFromListCommand ??= new global::CommunityToolkit.Mvvm.Input.RelayCommand<global::CommunityToolkitMvvmDemo.Person>(new global::System.Action<global::CommunityToolkitMvvmDemo.Person>(RemoveFromList));
```

```xml
<Button
    Command="{x:Bind ViewModel.RemoveFromListCommand}"
    CommandParameter="{x:Bind PeopleList.SelectedItem, Mode=OneWay}"
    Content="削除" />
```

また、`CanExecute`でコマンドの実行可否を制御し、

```cs
private bool CanAddPerson =>
    string.IsNullOrEmpty(FirstName) is false &&
    string.IsNullOrEmpty(LastName) is false;

[RelayCommand(CanExecute = nameof(CanAddPerson))]
private void AddPersonToList()
{
    var person = new Person(FirstName, LastName);
    People.Add(person);
    FirstName = string.Empty;
    LastName = string.Empty;
}
```

`NotifyCanExecuteChangedFor`属性でコマンド実行可否の変化をUIに通知することができます。

```cs
[ObservableProperty]
[NotifyPropertyChangedFor(nameof(FullName))]
[NotifyCanExecuteChangedFor(nameof(AddPersonToListCommand))]
private string _firstName = string.Empty;

[ObservableProperty]
[NotifyPropertyChangedFor(nameof(FullName))]
[NotifyCanExecuteChangedFor(nameof(AddPersonToListCommand))]
private string _lastName = string.Empty;
```

```xml
<Button
    Command="{x:Bind ViewModel.RemoveFromListCommand}"
    CommandParameter="{x:Bind PeopleList.SelectedItem, Mode=OneWay}"
    Content="削除"/>
```


さらに、次のような非同期メソッドの場合、

```cs
[RelayCommand]
private async Task LoadPeopleAsync()
{
    var people = await _someService.GetListAsync();
    People = new ObservableCollection<Person>(people);
}
```

にも対応し、非同期コマンドを生成してくれます。

```cs
/// <summary>The backing field for <see cref="LoadPeopleCommand"/>.</summary>
[global::System.CodeDom.Compiler.GeneratedCode("CommunityToolkit.Mvvm.SourceGenerators.RelayCommandGenerator", "8.2.0.0")]
private global::CommunityToolkit.Mvvm.Input.AsyncRelayCommand? loadPeopleCommand;
/// <summary>Gets an <see cref="global::CommunityToolkit.Mvvm.Input.IAsyncRelayCommand"/> instance wrapping <see cref="LoadPeopleAsync"/>.</summary>
[global::System.CodeDom.Compiler.GeneratedCode("CommunityToolkit.Mvvm.SourceGenerators.RelayCommandGenerator", "8.2.0.0")]
[global::System.Diagnostics.CodeAnalysis.ExcludeFromCodeCoverage]
public global::CommunityToolkit.Mvvm.Input.IAsyncRelayCommand LoadPeopleCommand => loadPeopleCommand ??= new global::CommunityToolkit.Mvvm.Input.AsyncRelayCommand(new global::System.Func<global::System.Threading.Tasks.Task>(LoadPeopleAsync));
```

さらにさらに、`IncludeCancelCommand=true`を設定することで、`CancellationToken`を引数として渡せるようになり、

```cs
[RelayCommand(IncludeCancelCommand = true)]
private async Task LoadPeopleAsync(CancellationToken cancellationToken)
{
    var people = await _someService.GetListAsync(cancellationToken);
    People = new ObservableCollection<Person>(people);
}
```

`LoadPeopleCommand`だけでなく、`LoadPeopleCancelCommand`も生成してくれますので、

```cs
/// <summary>The backing field for <see cref="LoadPeopleCommand"/>.</summary>
[global::System.CodeDom.Compiler.GeneratedCode("CommunityToolkit.Mvvm.SourceGenerators.RelayCommandGenerator", "8.2.0.0")]
private global::CommunityToolkit.Mvvm.Input.AsyncRelayCommand? loadPeopleCommand;
/// <summary>Gets an <see cref="global::CommunityToolkit.Mvvm.Input.IAsyncRelayCommand"/> instance wrapping <see cref="LoadPeopleAsync"/>.</summary>
[global::System.CodeDom.Compiler.GeneratedCode("CommunityToolkit.Mvvm.SourceGenerators.RelayCommandGenerator", "8.2.0.0")]
[global::System.Diagnostics.CodeAnalysis.ExcludeFromCodeCoverage]
public global::CommunityToolkit.Mvvm.Input.IAsyncRelayCommand LoadPeopleCommand => loadPeopleCommand ??= new global::CommunityToolkit.Mvvm.Input.AsyncRelayCommand(new global::System.Func<global::System.Threading.CancellationToken, global::System.Threading.Tasks.Task>(LoadPeopleAsync));

/// <summary>The backing field for <see cref="LoadPeopleCancelCommand"/>.</summary>
[global::System.CodeDom.Compiler.GeneratedCode("CommunityToolkit.Mvvm.SourceGenerators.RelayCommandGenerator", "8.2.0.0")]
private global::System.Windows.Input.ICommand? loadPeopleCancelCommand;
/// <summary>Gets an <see cref="global::System.Windows.Input.ICommand"/> instance that can be used to cancel <see cref="LoadPeopleCommand"/>.</summary>
[global::System.CodeDom.Compiler.GeneratedCode("CommunityToolkit.Mvvm.SourceGenerators.RelayCommandGenerator", "8.2.0.0")]
[global::System.Diagnostics.CodeAnalysis.ExcludeFromCodeCoverage]
public global::System.Windows.Input.ICommand LoadPeopleCancelCommand => loadPeopleCancelCommand ??= global::CommunityToolkit.Mvvm.Input.IAsyncRelayCommandExtensions.CreateCancelCommand(LoadPeopleCommand);
```

非同期処理のキャンセルに使うことができます。

```xml
<Button
    Command="{x:Bind ViewModel.LoadPeopleCommand}"
    Content="ロード開始" />
<Button
    Command="{x:Bind ViewModel.LoadPeopleCancelCommand}"
    Content="ロードキャンセル" />
```

以上です

Happy Coding!😊