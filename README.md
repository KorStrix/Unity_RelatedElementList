# Unity_RelatedElementList

이 프로젝는 인스펙터에서 커스텀 식을 작업할 수 있는 <br>
컬렉션 프로젝트와 그 예제입니다. <br>

예시는 총 3개가 있으며, <br>
사칙연산 계산기, Bool 계산기, <br>
Custom PC Input과 정의된 함수 연결 입니다. <br>

### Calculator
![](https://github.com/KorStrix/Unity_RelatedElementList/blob/master/ImageForGithub/Int_Calculator.gif?raw=true)

### Bool Calculator
![](https://github.com/KorStrix/Unity_RelatedElementList/blob/master/ImageForGithub/Bool_Calculator.gif?raw=true)

<br>

## 주 기능

**Element와 Element간의 관계를 담는 Collection입니다. <br>
사용자는 Element와 관계를 정의하여 직접 구현할 수 있습니다.** <br>

프로젝트 내부에 코드가 첨부되어 있지만, [링크](https://github.com/KorStrix/Unity_RelatedElementList/blob/master/Assets/StrixLibrary/Example/RelatedByElementsList/RelatedByElementsList_Example_Bool.cs) <br>
간단하게 설명하기 위해, Bool Calculator예시로 들겠습니다. <br>

```csharp
// Bool 과 Bool 사이의 관계를 정의합니다.
// SubString을 통해 간단하게 Display합니다.
enum EBoolCalculateType
{
    Nothing,
    [RegistSubString("&")]
    And,
    [RegistSubString("|")]
    Or,
    [RegistSubString("^")]
    Xor,
}

// 계산기를 정의합니다.
 // 계산기는 CRelateByOther를 상속받아야 합니다.
 [System.Serializable]
 public class BoolCalculator : CRelateByOther<BoolCalculator, EBoolCalculateType, bool>
 {
     public bool bValue;

     public override ValueDropdownList<EBoolCalculateType> IRelateByOther_GetEditorDisplayNameList()
     {
         return OdinExtension.GetValueDropDownList_EnumSubString<EBoolCalculateType>();
     }

     public override bool IRelateByOther_IsRequireOtherItem()
     {
         return pRelateType != EBoolCalculateType.Nothing;
     }

     public override string IRelateByOther_GetDisplayName()
     {
         return bValue.ToString();
     }

     public override bool GetResult()
     {
         return bValue;
     }

     public override string IRelateByOther_GetDisplayRelateName(EBoolCalculateType pRelateType)
     {
         return pRelateType.ToStringSub();
     }
 }

[System.Serializable]
public class BoolCalculator_List : RelatedByElementsListBase<BoolCalculator, EBoolCalculateType, bool>
{
    protected override bool Calculate_Relation(bool pResult_A, EBoolCalculateType pRelateType, bool pResult_B)
    {
        Debug.Log($"Calculate_Relation : {pResult_A} {pRelateType} {pResult_B}");

        switch (pRelateType)
        {
            case EBoolCalculateType.And: return pResult_A && pResult_B;
            case EBoolCalculateType.Or: return pResult_A || pResult_B;
            case EBoolCalculateType.Xor: return pResult_A ^ pResult_B;
        }

        return pResult_A;
    }
}

// 정의한 Calculator 필드 선언
public BoolCalculator_List listCalculator = new BoolCalculator_List();


// Unity는 기본적으로 Generic Class를 바로 Inspector에 출력할 수 없습니다.
// 그래서 Base Drawer를 상속받아야 합니다..
// 예시 코드는 Button 코드가 들어있지만, 빈 몸통으로 두어도 상관없습니다.

#if UNITY_EDITOR

[CustomPropertyDrawer(typeof(RelatedByElementsList_Example_Bool.BoolCalculator_List))]
public class BoolCalculator_List_Drawer : RelatedByElementsList_Drawer<RelatedByElementsList_Example_Bool.BoolCalculator, RelatedByElementsList_Example_Bool.EBoolCalculateType, bool>
{

}

#endif
```

<br>

## 인풋 시스템 예시
프로젝트에서 플레이를 누르고, 숫자키 1, 2 등을 누르면 로그가 출력됩니다. <br>
인풋 시스템은 Command와 Input으로 이루어진 List이며 각 요쇼의 설명은 하단과 같습니다. <br>

**(제가 임의로 네이밍 후 구현한 예시입니다.)**

### Command
커맨드는 프로그래머가 구현한 명령입니다. <br>
Init, Execute, Undo 등의 함수가 있으며, 반드시 구현해야 할 함수는 Execute 함수입니다. <br>

다음은 프로젝트에 있는 코드 예시입니다. [링크](https://github.com/KorStrix/Unity_RelatedElementList/blob/master/Assets/StrixLibrary/Example/RelatedByElementsList/CommandList_Example.cs)

<br>

```csharp
public class Command_PrintLog_1 : CommandBase
{
    public override void DoExcute(ref SInputValue sInputValue, ref bool bIsExcuted_DefaultIsTrue)
    {
        Debug.Log("1");
    }
}

public class Command_PrintLog_2 : CommandBase
{
    public override void DoExcute(ref SInputValue sInputValue, ref bool bIsExcuted_DefaultIsTrue)
    {
        Debug.Log("2");
    }
}
```

<br>
<br>

### Input
인풋은 여러 정의된 키보드 & 마우스 입력으로 이루어져있으며, <br>
각 입력과 입력 사이에 And, Or, Not 등의 관계를 묶을 수 있습니다. <br>
![](https://github.com/KorStrix/Unity_RelatedElementList/blob/master/ImageForGithub/InputEventSetting.gif?raw=true)

<br>

## 연락처
유니티 개발자 모임 카카오톡 & 디스코드 링크입니다.

- 카카오톡 : https://open.kakao.com/o/gOi17az
- 디스코드 : https://discord.gg/9BYFEbG
