---
标题：Ajax（远程数据）
元数据：
    描述：Select2广泛支持使用远程数据源填充下拉选项
分类：
    目录:docs
process:
    twig: true
never_cache_twig: true
---

 Select2使用jQuery的AJAX方法内置了AJAX支持。在本例中，我们可以用GitHub的API来搜寻存储库：
 
<select class="js-example-data-ajax form-control"></select>

**在您的HTML文件中:**

```
<select class="js-data-example-ajax"></select>
```

**在您的JavaScript文件中:**

```
$('.js-data-example-ajax').select2({
  ajax: {
    url: 'https://api.github.com/search/repositories',
    dataType: 'json'
    // 其他AJAX参数，添加到这里；请参考本章节末的完整例子
  }
});
```

您可以配置Select2如何使用`ajax`选项搜索远程数据。Select2会在`ajax`对象中传递任意选项到jQuery的 `$.ajax`函数，或着您定义的`transport`函数。

>>> 对于**仅远程数据源**，Select2直到该选项第一次被选中的时候才会创建一个新的`<option>`元素。这么做是由于性能的原因。一旦创建一个`<option>`，即使后续该选择被改变了，它也将一直停留在DOM中。

## 请求参数

当用户打开控件时，Select2将向指定的URL发出请求(除非配置了`minimumInputLength` )，并且每次用户在搜索框中键入时都将发出请求。默认情况下，它将发送以下查询字符串参数：

- `term` : 搜索框中当前搜索短语。
- `q`    : 包含与 `term`相同的内容。
- `_type`: “请求类型”。通常是`query`，但是在请求分页的情况下则为`query_append` 
- `page` :  请求当前页码。只支持分页（无限滚动）搜索。

例如，Select2可能发布一个类似这样的请求：`https://api.github.com/search/repositories?term=sel&_type=query&q=sel`

有时，您可能需要增加额外的查询参数到这个请求中。你可以通过重写`ajax.data`选项来修改请求发送的参数：

```
$('#mySelect2').select2({
  ajax: {
    url: 'https://api.github.com/orgs/select2/repos',
    data: function (params) {
      var query = {
        search: params.term,
        type: 'public'
      }

      // 查询参数为?search=[term]&type=public
      return query;
    }
  }
});
```

## 转换响应数据

您可以使用`ajax.processResults`选项来将API返回的数据转换成Select2期望的格式：

```
$('#mySelect2').select2({
  ajax: {
    url: '/example/api',
    processResults: function (data) {
      // 将响应对象的顶层'items'转换为'results'
      return {
        results: data.items
      };
    }
  }
});
```

>>> Select2期望在远程**服务器端**实现数据过滤。参考[此评论](https://github.com/select2/select2/issues/2321#issuecomment-42749687)来看为什么做这个应用选择的解释。如果服务器端的过滤不可实现，您可能会取而代之对使用Select2的[数据数组支持](/data-sources/arrays) 感兴趣。

##  默认（预选）值

控件设置预先选择的默认值，该控件从AJAX请求接收其数据。

为提供默认选项，您可以为每个选项引入一个`<option>`，其中包括应显示的值和文本:

```
<select class="js-example-data-ajax form-control">
  <option value="3620194" selected="selected">select2/select2</option>
</select>
```

为了以编程方式实现，您会需要[创建并附加一个新 `选项`](/programmatic-control/add-select-clear-items)。

## 分页

Select2为选项框外的远程数据源提供分页功能（“无限滚动”）。要使用该功能，您的远程数据源必须能够响应分页请求（像[Laravel](https://laravel.com/docs/5.5/pagination)和[UserFrosting](https://learn.userfrosting.com/database/data-sprunjing)这样的服务器端框架都内置了此功能）

要使用分页，您必须通过重写“ajax.data”设置，告诉Select2将任何必要的分页参数添加到request中。要检索的当前页面存储在`params.page`属性中。
```
$('#mySelect2').select2({
  ajax: {
    url: 'https://api.github.com/search/repositories',
    data: function (params) {
      var query = {
        search: params.term,
        page: params.page || 1
      }

      // 请求参数为?search=[term]&page=[page]
      return query;
    }
  }
});
```

Select2会在响应中设置一个`pagination.more`值。`more`的值应该是`true` 或着 `false`，告知Select2是否有还有更多的页面可供检索：

```
{
  "results": [
    {
      "id": 1,
      "text": "Option 1"
    },
    {
      "id": 2,
      "text": "Option 2"
    }
  ],
  "pagination": {
    "more": true
  }
}
```

如果您的服务器端代码没有在响应中生成`pagination.more`属性，您可以使用`processResults`来从其他可用的信息中生成这个值。比如，假设您的API返回了一个 `count_filtered` 值来告诉您在数据设置中总共多少（未分页的）结果可获得。如果您设置您的分页API一次返回10个结果，您可以与`count_filtered`的值一起使用来计算`pagination.more`的值：

```
processResults: function (data, params) {
    params.page = params.page || 1;

    return {
        results: data.results,
        pagination: {
            more: (params.page * 10) < data.count_filtered
        }
    };
}
```

## 请求限速

您可以告诉Select2在触发AJAX请求之前一直等待，直到用户输完它们的搜索条目。简单地用`ajax.delay`配置选项来告诉Select2在用户停止键入后，发送请求需要等待的时间：

```
$('#mySelect2').select2({
  ajax: {
    delay: 250 // 触发请求前等待 250 纳秒
  }
});
```

## 动态URLs

如果您的搜索结果没有单独的url，或者您需要调用一个函数来决定使用哪个的url，您可以为`ajax.url` 选项指定一个回调函数来生成url。当前的查询参数通过`params`选项传递进来。

```
$('#mySelect2').select2({
  ajax: {
    url: function (params) {
      return '/some/url/' + params.term;
    }
  }
});
```

## 其它传输方式

Select2使用在`ajax.transport`中定义的传输方法，将请求发给你给定的API。默认下此运输方式为`jQuery.ajax`，但也可以轻松重写：

```
$('#mySelect2').select2({
  ajax: {
    transport: function (params, success, failure) {
      var request = new AjaxRequest(params.url, params);
      request.on('success', success);
      request.on('failure', failure);
    }
  }
});
```

## jQuery `$.ajax`选项

所有传递到`ajax`的选项会被直接传递到`$.ajax`函数来执行AJAX请求。

Select2将会截取一些自定义选项，允许您根据需要自定义请求。

```
ajax: {
  // 在发出ajax请求之前，等待用户停止输入的毫秒数。
  delay: 250,
  // 您可以根据传入请求的参数创建自定义url。
  // 如果您正在使用一个基于JavaScript的框架来生成url请求，那么这将非常有用。
  // @param  包含用于生成请求的参数的对象。
  // @returns 请求的url。
  url: function (params) {
    return UrlGenerator.Random();
  },
  // 您可以根据用于发出请求的参数将自定义数据传入请求。
  // 对于`GET` 请求, 默认请求方法, 会将这些请求参数附加在url字符串后面
  // 对于`POST`请求， 会将这些请求参数以表单数据的格式传递给后端；
  // 对于其他请求类型，该函数返回的数据，必须基于jQuery 
  // 和您的服务器端要求的格式自定义。
  //
  // @param params 包含用于生成请求的参数的对象。
  // @returns 要直接传递到请求中的数据。
  data: function (params) {
    var queryParameters = {
      q: params.term
    }

    return queryParameters;
  },
  // 您可以修改从服务器返回的结果，允许您在最后时刻更改数据，或者找到需要传递// 到Select2正确部分。
  // 请记住，必须以对象数组方式传入
  //
  // @param data jQuery 直接返回的数据
  // @returns 包含结果数据的对象，同时还包含插件需要使用的其他元数据。该对象// 必须包含数据对象数组，作为`results` 键值。
  processResults: function (data) {
    return {
      results: data
    };
  },
  // 如果您不想使用jQuery提供的默认的AJAX传输函数，您可以自定义
  //
  // @param params 包含所有请求参数的对象。
  // @param success 回调函数，接收请求响应的结果`data` 
  // @param failure 回调函数，表示该请求未正常结束
  // @returns 一个包含`abort`函数的对象，如果需要，可以调用该函数终止请求。
  transport: function (params, success, failure) {
    var $request = $.ajax(params);

    $request.then(success);
    $request.fail(failure);

    return $request;
  }
}
```

## 附加的例子

这个代码驱动了在本章开篇时所展示的Github的例子。

<pre data-fill-from=".js-code-placeholder"></pre>

<script type="text/javascript" class="js-code-placeholder">

$(".js-example-data-ajax").select2({
  ajax: {
    url: "https://api.github.com/search/repositories",
    dataType: 'json',
    delay: 250,
    data: function (params) {
      return {
        q: params.term, // 检索词
        page: params.page
      };
    },
    processResults: function (data, params) {
      // 将结果解析为Select2所期望的格式，因为我们使用自定义的格式化函数
      // 我们不需要修改远程JSON数据，只需要实现无限滚动
      params.page = params.page || 1;

      return {
        results: data.items,
        pagination: {
          more: (params.page * 30) < data.total_count
        }
      };
    },
    cache: true
  },
  placeholder: 'Search for a repository',
  escapeMarkup: function (markup) { return markup; }, // 使得我们自定义格式化程序运行
  minimumInputLength: 1,
  templateResult: formatRepo,
  templateSelection: formatRepoSelection
});

function formatRepo (repo) {
  if (repo.loading) {
    return repo.text;
  }

  var markup = "<div class='select2-result-repository clearfix'>" +
    "<div class='select2-result-repository__avatar'><img src='" + repo.owner.avatar_url + "' /></div>" +
    "<div class='select2-result-repository__meta'>" +
      "<div class='select2-result-repository__title'>" + repo.full_name + "</div>";

  if (repo.description) {
    markup += "<div class='select2-result-repository__description'>" + repo.description + "</div>";
  }

  markup += "<div class='select2-result-repository__statistics'>" +
    "<div class='select2-result-repository__forks'><i class='fa fa-flash'></i> " + repo.forks_count + " Forks</div>" +
    "<div class='select2-result-repository__stargazers'><i class='fa fa-star'></i> " + repo.stargazers_count + " Stars</div>" +
    "<div class='select2-result-repository__watchers'><i class='fa fa-eye'></i> " + repo.watchers_count + " Watchers</div>" +
  "</div>" +
  "</div></div>";

  return markup;
}

function formatRepoSelection (repo) {
  return repo.full_name || repo.text;
}
</script>
