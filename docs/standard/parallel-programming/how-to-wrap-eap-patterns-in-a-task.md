---
title: 如何：在任务中包装 EAP 模式
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- tasks, how to wrap EAP patterns
ms.assetid: f11ed467-af2f-4504-8a2e-299a6c36d44e
ms.openlocfilehash: eab94ac91be0c755a1da74e2f2220e3b76cc4249
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/02/2020
ms.locfileid: "84290780"
---
# <a name="how-to-wrap-eap-patterns-in-a-task"></a>如何：在任务中包装 EAP 模式
下面的示例演示如何通过使用 <xref:System.Threading.Tasks.TaskCompletionSource%601> 将任意基于事件的异步模式 (EAP) 操作序列公开为一个任务。 此示例还展示了如何使用 <xref:System.Threading.CancellationToken> 对 <xref:System.Net.WebClient> 对象调用内置的取消方法。  
  
## <a name="example"></a>示例  
 [!code-csharp[FromAsync#08](../../../samples/snippets/csharp/VS_Snippets_Misc/fromasync/cs/fromasync.cs#08)]
 [!code-vb[FromAsync#08](../../../samples/snippets/visualbasic/VS_Snippets_Misc/fromasync/vb/module1.vb#08)]  
  
## <a name="see-also"></a>另请参阅

- [TPL 和传统 .NET Framework 异步编程](tpl-and-traditional-async-programming.md)
