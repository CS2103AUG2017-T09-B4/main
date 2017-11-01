# NabeelZaheer
###### \java\seedu\address\logic\commands\AddTagCommandTest.java
``` java
/**
 * Contains integration tests (interaction with the Model) and unit tests for {@code AddTagCommand}.
 */
public class AddTagCommandTest {
    private Model model = new ModelManager(getTypicalAddressBook(), new UserPrefs());

    @Test
    public void execute_addValidTagValidIndex_success() throws Exception {
        Tag tagToAdd = new Tag("enemy");
        Set<Index> indexSet = new HashSet<>();
        Set<Tag> tagSet = new HashSet<>();
        tagSet.add(tagToAdd);
        indexSet.add(INDEX_FIRST_PERSON);

        AddTagCommand addTagCommand = prepareCommand(tagSet, indexSet, "1");

        String expectedMessage = String.format(AddTagCommand.MESSAGE_ADDED_SUCCESS + " to index "
                + "1.", tagSet);

        ModelManager expectedModel = new ModelManager(model.getAddressBook(), new UserPrefs());
        expectedModel.addTag(tagSet, indexSet);

        assertCommandSuccess(addTagCommand, model, expectedMessage, expectedModel);
    }

    @Test
    public void execute_addValidOutOfBoundsIndex_throwsCommandException() throws Exception {
        Tag tagToAdd = new Tag("enemy");
        Set<Index> indexSet = new HashSet<>();
        Set<Tag> tagSet = new HashSet<>();
        tagSet.add(tagToAdd);
        indexSet.add(Index.fromOneBased(model.getFilteredPersonList().size() + 1));
        String indexInput = String.valueOf(model.getFilteredPersonList().size() + 1);

        AddTagCommand addTagCommand = prepareCommand(tagSet, indexSet, indexInput);

        String expectedMessage =  Messages.MESSAGE_INVALID_PERSON_DISPLAYED_INDEX;

        assertCommandFailure(addTagCommand, model, expectedMessage);
    }

    @Test
    public void execute_validTagMultipleIndexes_success() throws Exception {
        Tag tagToAdd = new Tag("enemy");
        Set<Index> indexSet = new HashSet<>();
        Set<Tag> tagSet = new HashSet<>();
        tagSet.add(tagToAdd);
        indexSet.add(INDEX_FIRST_PERSON);
        indexSet.add(INDEX_SECOND_PERSON);

        AddTagCommand addTagCommand = prepareCommand(tagSet, indexSet, "1, 2");

        String expectedMessage = String.format(AddTagCommand.MESSAGE_ADDED_SUCCESS + " to index "
                + "1, 2.", tagSet);

        ModelManager expectedModel = new ModelManager(model.getAddressBook(), new UserPrefs());
        expectedModel.addTag(tagSet, indexSet);

        assertCommandSuccess(addTagCommand, model, expectedMessage, expectedModel);
    }

    @Test
    public void execute_multipleTagsValidIndex_success() throws Exception {
        Tag tagToAdd1 = new Tag("enemy");
        Tag tagToAdd2 = new Tag("brother");
        Set<Index> indexSet = new HashSet<>();
        Set<Tag> tagSet = new HashSet<>();
        tagSet.add(tagToAdd1);
        tagSet.add(tagToAdd2);
        indexSet.add(INDEX_FIRST_PERSON);

        AddTagCommand addTagCommand = prepareCommand(tagSet, indexSet, "1");

        String expectedMessage = String.format(AddTagCommand.MESSAGE_ADDED_SUCCESS + " to index "
                + "1.", tagSet);

        ModelManager expectedModel = new ModelManager(model.getAddressBook(), new UserPrefs());
        expectedModel.addTag(tagSet, indexSet);

        assertCommandSuccess(addTagCommand, model, expectedMessage, expectedModel);
    }

    @Test
    public void execute_addDuplicateTag_throwsCommandException() throws Exception {
        Tag tagToAdd = new Tag("friends");
        Set<Index> indexSet = new HashSet<>();
        Set<Tag> tagSet = new HashSet<>();
        tagSet.add(tagToAdd);
        indexSet.add(INDEX_FIRST_PERSON);

        AddTagCommand addTagCommand = prepareCommand(tagSet, indexSet, "1");
        assertCommandFailure(addTagCommand, model, String.format(
                AddTagCommand.MESSAGE_DUPLICATE_TAG + " index: " + INDEX_FIRST_PERSON.getOneBased()
                        + ".", tagSet));
    }

    @Test
    public void equals() throws Exception {
        Tag firstTag = new Tag("friends");
        Tag secondTag = new Tag("owesMoney");
        Set<Index> indexSet1 = new HashSet<>();
        Set<Index> indexSet2 = new HashSet<>();
        Set<Tag> tagSet1 = new HashSet<>();
        Set<Tag> tagSet2 = new HashSet<>();
        tagSet1.add(firstTag);
        tagSet2.add(secondTag);
        indexSet1.add(INDEX_FIRST_PERSON);
        indexSet2.add(INDEX_SECOND_PERSON);

        AddTagCommand addTagCommand1 = new AddTagCommand(tagSet1, indexSet1, "1");
        AddTagCommand addTagCommand2 = new AddTagCommand(tagSet1, indexSet2, "2");
        AddTagCommand addTagCommand3 = new AddTagCommand(tagSet2, indexSet1, "1");

        // same object -> returns true
        assertTrue(addTagCommand1.equals(addTagCommand1));

        // different types -> returns false
        assertFalse(addTagCommand1.equals(new ClearCommand()));

        // null -> returns false
        assertFalse(addTagCommand1.equals(null));

        // different tag -> returns false (same Index)
        assertFalse(addTagCommand1.equals(addTagCommand3));

        // different index -> returns false (same Tag)
        assertFalse(addTagCommand1.equals(addTagCommand2));

        // different index -> returns false (different Tag)
        assertFalse(addTagCommand2.equals(addTagCommand3));
    }


    /**
     * Returns a {@code AddTagCommand}.
     */
    private AddTagCommand prepareCommand(Set<Tag> tag, Set<Index> index, String indexInput) {
        AddTagCommand addTagCommand = new AddTagCommand(tag, index, indexInput);
        addTagCommand.setData(model, new CommandHistory(), new UndoRedoStack());
        return addTagCommand;
    }
}
```
###### \java\seedu\address\logic\commands\FindCommandTest.java
``` java
    @Test
    public void equals() {
        List<String> keywords1 = Arrays.asList("first", "third");
        List<String> keywords2 = new ArrayList<>();
        List<String> keywords3 = new ArrayList<>();
        List<String> keywords4 = new ArrayList<>();
        List<String> keywords5 = new ArrayList<>();
        List<List<String>> list1 = new ArrayList<>();
        list1.add(keywords1);
        list1.add(keywords2);
        list1.add(keywords3);
        list1.add(keywords4);
        list1.add(keywords5);

        List<String> keywordsA = Arrays.asList("second", "fourth");
        List<String> keywordsB = new ArrayList<>();
        List<String> keywordsC = new ArrayList<>();
        List<String> keywordsD = new ArrayList<>();
        List<String> keywordsE = new ArrayList<>();
        List<List<String>> list2 = new ArrayList<>();
        list2.add(keywordsA);
        list2.add(keywordsB);
        list2.add(keywordsC);
        list2.add(keywordsD);
        list2.add(keywordsE);

        FieldContainsKeywordsPredicate firstPredicate =
                new FieldContainsKeywordsPredicate(list1);
        FieldContainsKeywordsPredicate secondPredicate =
                new FieldContainsKeywordsPredicate(list2);


        FindCommand findFirstCommand = new FindCommand(firstPredicate);
        FindCommand findSecondCommand = new FindCommand(secondPredicate);


        // same object -> returns true
        assertTrue(findFirstCommand.equals(findFirstCommand));


        // same values -> returns true
        FindCommand findFirstCommandCopy = new FindCommand(firstPredicate);
        assertTrue(findFirstCommand.equals(findFirstCommandCopy));

        // different types -> returns false
        assertFalse(findFirstCommand.equals(1));

        // null -> returns false
        assertFalse(findFirstCommand.equals(null));

        // different person -> returns false
        assertFalse(findFirstCommand.equals(findSecondCommand));
    }

    @Test
    public void execute_keyword_noPersonFound() throws Exception {
        String expectedMessage = String.format(MESSAGE_PERSONS_LISTED_OVERVIEW, 0);
        FindCommand command = prepareCommand(" n/Bob");
        assertCommandSuccess(command, expectedMessage, Collections.emptyList());
    }

    @Test
    public void execute_multipleKeywords_multiplePersonsFound() throws Exception {
        String expectedMessage = String.format(MESSAGE_PERSONS_LISTED_OVERVIEW, 3);
        FindCommand command = prepareCommand(" n/Kurz n/Elle n/Kunz");
        assertCommandSuccess(command, expectedMessage, Arrays.asList(CARL, ELLE, FIONA));
    }

    @Test
    public void execute_multipleTagKeywords_multiplePersonFound() throws Exception {
        String expectedMessage = String.format(MESSAGE_PERSONS_LISTED_OVERVIEW, 1);
        FindCommand command = prepareCommand(" t/owesMoney");
        assertCommandSuccess(command, expectedMessage, Arrays.asList(BENSON));
    }
```
###### \java\seedu\address\logic\commands\RemoveCommandTest.java
``` java
/**
 * Contains integration tests (interaction with the Model) and unit tests for {@code RemoveTagCommand}.
 */
public class RemoveCommandTest {
    private Model model = new ModelManager(getTypicalAddressBook(), new UserPrefs());

    @Test
    public void execute_removeValidTagAll_success() throws Exception {
        Tag tagToRemove = new Tag("friends");
        Set<Index> indexSet = new HashSet<>();
        Set<Tag> tagSet = new HashSet<>();
        tagSet.add(tagToRemove);
        List<String> indexDisplay = new ArrayList<>();

        RemoveTagCommand removeCommand = prepareCommand(tagSet, indexSet, indexDisplay);

        String expectedMessage = String.format(RemoveTagCommand.MESSAGE_REMOVE_SUCCESS
                        + " from address book.", tagSet);

        ModelManager expectedModel = new ModelManager(model.getAddressBook(), new UserPrefs());
        expectedModel.removeTag(tagSet, indexDisplay);

        assertCommandSuccess(removeCommand, model, expectedMessage, expectedModel);
    }

    @Test
    public void execute_removeInvalidTagAll_throwsCommandException() throws Exception {
        Tag tagToRemove = new Tag("enemy");
        Set<Index> indexSet = new HashSet<>();
        Set<Tag> tagSet = new HashSet<>();
        tagSet.add(tagToRemove);
        List<String> indexDisplay = new ArrayList<>();

        RemoveTagCommand removeCommand = prepareCommand(tagSet, indexSet, indexDisplay);

        String expectedMessage =  String.format(
                RemoveTagCommand.MESSAGE_TAG_NOT_FOUND + " the address book.", tagSet);

        assertCommandFailure(removeCommand, model, expectedMessage);
    }

    @Test
    public void execute_validTagValidIndexUnfilteredList_success() throws Exception {
        Tag tagToRemove = new Tag("friends");
        Set<Index> indexSet = new HashSet<>();
        Set<Tag> tagSet = new HashSet<>();
        tagSet.add(tagToRemove);
        indexSet.add(INDEX_FIRST_PERSON);
        List<String> indexDisplay = new ArrayList<>();
        indexDisplay.add("1");

        RemoveTagCommand removeCommand = prepareCommand(tagSet, indexSet, indexDisplay);

        String expectedMessage = String.format(RemoveTagCommand.MESSAGE_REMOVE_SUCCESS + " from index "
                + INDEX_FIRST_PERSON.getOneBased() + ".", tagSet);

        ModelManager expectedModel = new ModelManager(model.getAddressBook(), new UserPrefs());
        expectedModel.removeTag(tagSet, indexDisplay);

        assertCommandSuccess(removeCommand, model, expectedMessage, expectedModel);
    }

    @Test
    public void execute_validTagInvalidIndexUnfilteredList_throwsCommandException() throws Exception {
        Tag tagToRemove = new Tag("friends");
        Index outOfBoundIndex = Index.fromOneBased(model.getFilteredPersonList().size() + 1);
        Set<Index> indexSet = new HashSet<>();
        Set<Tag> tagSet = new HashSet<>();
        tagSet.add(tagToRemove);
        indexSet.add(outOfBoundIndex);
        String indexString = String.valueOf(model.getFilteredPersonList().size() + 1);
        List<String> indexDisplay = new ArrayList<>();
        indexDisplay.add(indexString);

        RemoveTagCommand removeCommand = prepareCommand(tagSet, indexSet, indexDisplay);
        assertCommandFailure(removeCommand, model, Messages.MESSAGE_INVALID_PERSON_DISPLAYED_INDEX);
    }

    @Test
    public void execute_invalidTagValidIndexUnfilteredList_throwsCommandException() throws Exception {
        Tag tagToRemove = new Tag("enemy");
        Set<Index> indexSet = new HashSet<>();
        Set<Tag> tagSet = new HashSet<>();
        tagSet.add(tagToRemove);
        indexSet.add(INDEX_FIRST_PERSON);
        List<String> indexDisplay = new ArrayList<>();
        indexDisplay.add("1");

        RemoveTagCommand removeCommand = prepareCommand(tagSet, indexSet, indexDisplay);
        assertCommandFailure(removeCommand, model, String.format(
                RemoveTagCommand.MESSAGE_TAG_NOT_FOUND + " index: " + INDEX_FIRST_PERSON.getOneBased() + ".", tagSet));
    }

    @Test
    public void equals() throws Exception {
        Tag firstTag = new Tag("friends");
        Tag secondTag = new Tag("owesMoney");
        Set<Index> indexSet1 = new HashSet<>();
        Set<Tag> tagSet1 = new HashSet<>();
        tagSet1.add(firstTag);
        indexSet1.add(INDEX_FIRST_PERSON);
        Set<Index> indexSet2 = new HashSet<>();
        Set<Tag> tagSet2 = new HashSet<>();
        tagSet2.add(secondTag);
        indexSet2.add(INDEX_SECOND_PERSON);
        Set<Index> emptySet = new HashSet<>();
        List<String> indexDisplay = new ArrayList<>();
        indexDisplay.add("");
        List<String> indexDisplay1 = new ArrayList<>();
        indexDisplay1.add("1");
        List<String> indexDisplay2 = new ArrayList<>();
        indexDisplay2.add("2");
        RemoveTagCommand removeFirstCommand = new RemoveTagCommand(tagSet1, emptySet, indexDisplay);
        RemoveTagCommand removeSecondCommand = new RemoveTagCommand(tagSet2, emptySet, indexDisplay);
        RemoveTagCommand removeThirdCommand = new RemoveTagCommand(tagSet1, indexSet1, indexDisplay1);
        RemoveTagCommand removeFourthCommand = new RemoveTagCommand(tagSet1, indexSet2, indexDisplay2);
        RemoveTagCommand removeFifthCommand = new RemoveTagCommand(tagSet2, indexSet1, indexDisplay1);

        // same object -> returns true (no Index)
        assertTrue(removeFirstCommand.equals(removeFirstCommand));

        // same object -> returns true (with Index)
        assertTrue(removeThirdCommand.equals(removeThirdCommand));

        // different types -> returns false
        assertFalse(removeFirstCommand.equals(new ClearCommand()));

        // null -> returns false
        assertFalse(removeFirstCommand.equals(null));

        // different tag -> returns false (no Index)
        assertFalse(removeFirstCommand.equals(removeSecondCommand));

        // different tag -> returns false (with Index)
        assertFalse(removeThirdCommand.equals(removeFifthCommand));

        // different index -> returns false (same Tag)
        assertFalse(removeThirdCommand.equals(removeFourthCommand));

        // different index -> returns false (different Tag)
        assertFalse(removeFourthCommand.equals(removeFifthCommand));
    }


    /**
     * Returns a {@code RemoveTagCommand}.
     */
    private RemoveTagCommand prepareCommand(Set<Tag> tag, Set<Index> index, List<String> indexDisplay) {
        RemoveTagCommand removeCommand = new RemoveTagCommand(tag, index, indexDisplay);
        removeCommand.setData(model, new CommandHistory(), new UndoRedoStack());
        return removeCommand;
    }
}
```
###### \java\seedu\address\logic\parser\AddressBookParserTest.java
``` java
    @Test
    public void parseCommand_remove() throws Exception {
        Tag tag = new Tag("friends");
        RemoveTagCommand command = (RemoveTagCommand) parser.parseCommand(
                RemoveTagCommand.COMMAND_WORD + " friends 1");
        Set<Index> indexSet = new HashSet<>();
        Set<Tag> tagSet = new HashSet<>();
        tagSet.add(tag);
        indexSet.add(INDEX_FIRST_PERSON);
        List<String> indexDisplay = new ArrayList<>();
        indexDisplay.add("1");
        assertEquals(new RemoveTagCommand(tagSet, indexSet, indexDisplay), command);
    }

    @Test
    public void parseCommand_addTag() throws Exception {
        Tag tag = new Tag("enemy");
        AddTagCommand command = (AddTagCommand) parser.parseCommand(
                AddTagCommand.COMMAND_WORD + " enemy 1");
        Set<Index> indexSet = new HashSet<>();
        Set<Tag> tagSet = new HashSet<>();
        tagSet.add(tag);
        indexSet.add(INDEX_FIRST_PERSON);
        assertEquals(new AddTagCommand(tagSet, indexSet, "1"), command);
    }
```
###### \java\seedu\address\logic\parser\AddTagCommandParserTest.java
``` java
public class AddTagCommandParserTest {

    private AddTagCommandParser parser = new AddTagCommandParser();

    @Test
    public void parse_validArgsWithIndex_returnsAddTagCommand() throws Exception {
        Tag tagToAdd = new Tag("enemy");
        Set<Index> indexSet = new HashSet<>();
        Set<Tag> tagSet = new HashSet<>();
        tagSet.add(tagToAdd);
        indexSet.add(INDEX_SECOND_PERSON);
        AddTagCommand addTagCommand = new AddTagCommand(tagSet, indexSet, "2");
        assertParseSuccess(parser, "enemy 2", addTagCommand);
    }

    @Test
    public void parse_validArgsWithIndexes_returnsAddTagCommand() throws Exception {
        Tag tagToAdd = new Tag("enemy");
        Set<Index> indexSet = new HashSet<>();
        Set<Tag> tagSet = new HashSet<>();
        tagSet.add(tagToAdd);
        indexSet.add(INDEX_FIRST_PERSON);
        indexSet.add(INDEX_SECOND_PERSON);
        AddTagCommand addTagCommand = new AddTagCommand(tagSet, indexSet, "1, 2");
        assertParseSuccess(parser, "enemy 1 2", addTagCommand);
    }

    @Test
    public void parse_invalidArgsNoIndex_throwsParseException() {
        assertParseFailure(parser, "?", String.format(MESSAGE_INVALID_COMMAND_FORMAT,
                AddTagCommand.MESSAGE_USAGE));
    }

    @Test
    public void parse_invalidArgsWithIndex_throwsParseException() {
        assertParseFailure(parser, "? 2", String.format(MESSAGE_INVALID_COMMAND_FORMAT,
                AddTagCommand.MESSAGE_USAGE));
    }

    @Test
    public void parse_validArgsWithInvalidIndex_throwsParseException() {
        assertParseFailure(parser, "friends 0", String.format(MESSAGE_INVALID_COMMAND_FORMAT,
                AddTagCommand.MESSAGE_USAGE));
    }

    @Test
    public void parse_emptyArgs_throwsParseException() {
        assertParseFailure(parser, "", String.format(
                MESSAGE_INVALID_COMMAND_FORMAT, AddTagCommand.MESSAGE_USAGE));
    }

    @Test
    public void parse_invalidArgsOrder_throwsParseException() {
        assertParseFailure(parser, "enemy 1 wendy", String.format(
                MESSAGE_INVALID_COMMAND_FORMAT, AddTagCommand.MESSAGE_USAGE));
    }

    @Test
    public void parse_validArgsEmptyIndex_throwsParseException() {
        assertParseFailure(parser, "enemy", "Please provide at least one index.\n"
                + String.format(MESSAGE_INVALID_COMMAND_FORMAT, AddTagCommand.MESSAGE_USAGE));
    }

}
```
###### \java\seedu\address\logic\parser\FindCommandParserTest.java
``` java
    @Test
    public void parse_validNameArgs_returnsFindCommand() {
        // no leading and trailing whitespaces
        List<String> keywords1 = Arrays.asList("Alice", "Bob");
        List<String> keywords2 = new ArrayList<>();
        List<String> keywords3 = new ArrayList<>();
        List<String> keywords4 = new ArrayList<>();
        List<String> keywords5 = new ArrayList<>();
        List<List<String>> list = new ArrayList<>();
        list.add(keywords1);
        list.add(keywords2);
        list.add(keywords3);
        list.add(keywords4);
        list.add(keywords5);
        FindCommand expectedFindCommand =
                new FindCommand(new FieldContainsKeywordsPredicate(list));
        assertParseSuccess(parser, " n/Alice n/Bob", expectedFindCommand);
    }

    @Test
    public void parse_missingInput_throwsParseException() {
        // multiple whitespaces between keywords
        assertParseFailure(parser, " n/Alice t/      ", "Missing input for field: t/ \n"
                + FindCommand.MESSAGE_USAGE);
    }

    @Test
    public void parse_invalidPrefix_throwsParseException() {
        assertParseFailure(parser, "alex", String.format(MESSAGE_INVALID_COMMAND_FORMAT,
                FindCommand.MESSAGE_USAGE));
    }


}
```
###### \java\seedu\address\logic\parser\RemoveCommandParserTest.java
``` java
public class RemoveCommandParserTest {

    private RemoveCommandParser parser = new RemoveCommandParser();

    @Test
    public void parse_validArgsWithIndex_returnsRemoveTagCommand() throws Exception {
        Tag tagToRemove = new Tag("friends");
        Set<Index> indexSet = new HashSet<>();
        Set<Tag> tagSet = new HashSet<>();
        tagSet.add(tagToRemove);
        indexSet.add(INDEX_SECOND_PERSON);
        List<String> indexDisplay = new ArrayList<>();
        indexDisplay.add("2");
        RemoveTagCommand removeCommand = new RemoveTagCommand(tagSet, indexSet, indexDisplay);
        assertParseSuccess(parser, "friends 2", removeCommand);
    }

    @Test
    public void parse_validArgsNoIndex_returnsRemoveTagCommand() throws Exception {
        Tag tagToRemove = new Tag("friends");
        Set<Index> indexSet = new HashSet<>();
        Set<Tag> tagSet = new HashSet<>();
        tagSet.add(tagToRemove);
        List<String> indexDisplay = new ArrayList<>();
        indexDisplay.add("");
        RemoveTagCommand removeCommand = new RemoveTagCommand(tagSet, indexSet, indexDisplay);
        assertParseSuccess(parser, "friends", removeCommand);
    }

    @Test
    public void parse_invalidArgsNoIndex_throwsParseException() {
        assertParseFailure(parser, "?", String.format(MESSAGE_INVALID_COMMAND_FORMAT,
                RemoveTagCommand.MESSAGE_USAGE));
    }

    @Test
    public void parse_invalidArgsWithIndex_throwsParseException() {
        assertParseFailure(parser, "? 2", String.format(MESSAGE_INVALID_COMMAND_FORMAT,
                RemoveTagCommand.MESSAGE_USAGE));
    }

    @Test
    public void parse_validArgsWithInvalidIndex_throwsParseException() {
        assertParseFailure(parser, "friends 0", String.format(MESSAGE_INVALID_COMMAND_FORMAT,
                RemoveTagCommand.MESSAGE_USAGE));
    }

}
```